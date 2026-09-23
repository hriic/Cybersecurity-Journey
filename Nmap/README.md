# Nmap — Network Discovery & Enumeration

> These are my structured notes from hands-on penetration-testing labs. The goal is not to memorize flags, but to understand what question each scan answers and how its result changes the next enumeration step.

Nmap (Network Mapper) is a network discovery and security-auditing tool. During an authorized penetration test, I mainly use it to answer four questions:

1. **Which hosts are reachable?**
2. **Which ports are exposed?**
3. **Which services and versions are behind those ports?**
4. **What additional information can I safely enumerate from those services?**

A scan is only the beginning of enumeration. An open port is evidence that tells me where to investigate next.

---

## 1. The mental model

My basic workflow is:

```text
Scope
  ↓
Host Discovery
  ↓
Port Discovery
  ↓
Service / Version Detection
  ↓
NSE / Protocol-specific Enumeration
  ↓
Research & Validation
  ↓
Document Findings
```

For example, if TCP/80 is open, I do not immediately assume a vulnerability. I first identify the web server, inspect the application, enumerate content and understand the technology stack.

---

## 2. Target specification

Nmap can scan one host, several hosts, or an authorized subnet.

```bash
nmap 10.10.10.10
nmap 10.10.10.10 10.10.10.20
nmap 10.10.10.0/24
```

CIDR notation matters. A `/24` represents 256 IPv4 addresses in that network range, although usable-host concepts and what Nmap actually receives responses from are separate questions.

Always confirm the permitted scope before scanning.

---

## 3. Host discovery

Before spending time scanning every port, I can determine which hosts appear reachable.

```bash
nmap -sn 10.10.10.0/24
```

`-sn` performs host discovery without a normal port scan.

### ICMP discovery

```bash
nmap -PE TARGET
nmap -PP TARGET
nmap -PM TARGET
```

| Option | Probe |
|---|---|
| `-PE` | ICMP Echo Request |
| `-PP` | ICMP Timestamp Request |
| `-PM` | ICMP Address Mask Request |

A host not responding to one probe does **not** necessarily mean it is offline. Firewalls can block ICMP or particular probe types.

### TCP discovery

TCP probes can help when ICMP is filtered.

```bash
nmap -PS80 TARGET
nmap -PA80 TARGET
```

`-PS` uses a TCP SYN probe and `-PA` uses a TCP ACK probe. The port can be selected based on what traffic is likely to be permitted.

### Skip discovery: -Pn

```bash
nmap -Pn TARGET
```

`-Pn` tells Nmap to treat the target as online and proceed with scanning rather than relying on the normal host-discovery phase. I use it when a known lab target appears "down" because discovery probes are being filtered.

---

## 4. TCP port scanning

Ports identify network endpoints exposed by the target.

### Default scan

```bash
nmap TARGET
```

Nmap's default behavior checks a commonly used set of TCP ports. This is useful for a quick first look, but it is not equivalent to checking every TCP port.

### Specific ports

```bash
nmap -p 22 TARGET
nmap -p 22,80,443 TARGET
nmap -p 1-1000 TARGET
```

### All TCP ports

```bash
nmap -p- TARGET
```

`-p-` scans TCP ports 1–65535. This matters because services can listen on unusual ports such as 8080, 8443 or high-numbered custom ports.

---

## 5. SYN scan vs TCP Connect scan

### SYN scan — -sS

```bash
sudo nmap -sS TARGET
```

A SYN scan starts the TCP handshake by sending SYN. An open port normally responds with SYN/ACK, while a closed port normally responds with RST. Nmap can then infer the state without completing a normal application connection.

Conceptually:

```text
Scanner                    Target
   | ----- SYN ------------> |
   | <---- SYN/ACK ---------- |  Open
   | ----- RST -------------> |
```

### TCP Connect scan — -sT

```bash
nmap -sT TARGET
```

`-sT` asks the operating system to establish a normal TCP connection. It is useful when raw-packet privileges required by some scan types are unavailable.

The important part for me is not memorizing that one is "better"; it is understanding what packets are sent and why Nmap reaches its conclusion.

---

## 6. Understanding port states

### open
An application is accepting connections on that port.

### closed
The host is reachable, but nothing is listening on that port.

### filtered
Nmap cannot determine whether the port is open because filtering prevents the expected response.

Other states such as `unfiltered`, `open|filtered`, and `closed|filtered` can appear depending on the scan technique and responses.

This distinction matters. **Filtered is not the same as closed.**

---

## 7. Service and version detection — -sV

Finding port 80 is useful. Knowing what is actually running there is much more useful.

```bash
nmap -sV TARGET
```

Example-style result:

```text
PORT   STATE OPEN SERVICE VERSION
22/tcp open  ssh     OpenSSH ...
80/tcp open  http    Apache httpd ...
```

Version detection sends probes and analyzes responses to identify the application and, where possible, its version.

A version string is a **lead**, not proof of vulnerability. Packages can be patched without changing an obvious banner, versions can be hidden, and configuration matters.

---

## 8. Default NSE scripts — -sC

```bash
nmap -sC TARGET
```

`-sC` runs Nmap's default NSE script set. These scripts can collect useful service-specific information.

A common enumeration pass in my labs is:

```bash
nmap -sC -sV TARGET
```

I read that command as:

> Detect the services and versions, then run the default scripts that may provide additional context.

I do **not** treat `-sC -sV` as a magic command that replaces manual enumeration.

---

## 9. UDP scanning

TCP and UDP are different transport protocols, so a TCP-only scan can miss important services.

```bash
sudo nmap -sU TARGET
```

Common UDP-based services can include DNS (53), SNMP (161) and others.

UDP enumeration can be slower and interpretation can be less straightforward because UDP does not use the same handshake behavior as TCP.

A targeted UDP scan is often useful:

```bash
sudo nmap -sU -p 53,161 TARGET
```

---

## 10. Fast scan — -F

```bash
nmap -F TARGET
```

`-F` reduces the number of ports checked compared with Nmap's normal default port selection. I use it when I specifically want a quick initial picture, not when completeness matters.

---

## 11. Why did Nmap report that state? — --reason

```bash
sudo nmap -sS -F --reason TARGET
```

`--reason` shows the reason Nmap assigned a host or port state.

This is particularly useful while learning because it connects:

```text
Packet received
      ↓
Nmap interpretation
      ↓
Reported state
```

Instead of only seeing `open`, I can understand what response led Nmap to that conclusion.

---

## 12. Nmap Scripting Engine (NSE)

NSE allows Nmap to run Lua-based scripts for tasks such as discovery and service enumeration.

On my Kali/AttackBox-style environments, scripts are commonly available under:

```text
/usr/share/nmap/scripts/
```

### Run one script

```bash
nmap --script SCRIPT_NAME TARGET
```

### Pass script arguments

```bash
nmap --script SCRIPT_NAME --script-args 'key=value' TARGET
```

Before running a script, I check its description and understand what it sends to the target.

Useful ways to inspect locally:

```bash
nmap --script-help SCRIPT_NAME
grep -R "keyword" /usr/share/nmap/scripts/
```

I have practiced with HTTP and SSH-oriented NSE scripts, including learning what `http-robots.txt` and `ssh2-enum-algos` enumerate.

---

## 13. Service-oriented enumeration

Nmap tells me **where to look**. The next step depends on the protocol.

### HTTP / HTTPS

If I find:

```text
80/tcp   open http
443/tcp  open https
8080/tcp open http
```

I investigate the web application: title, headers, technologies, directories, virtual hosts, authentication and application behavior.

### SMB — 139/445

I think about shares, domain/workgroup information, access permissions and SMB-specific enumeration.

### FTP — 21

I investigate the server/version and whether the authorized lab exposes anonymous or credentialed access.

### SSH — 22

I identify the implementation/version and configuration information available to me. An exposed SSH port by itself is not a vulnerability.

### DNS — 53

I determine whether DNS is TCP/UDP accessible and what DNS-specific enumeration is appropriate for the permitted environment.

The principle is:

```text
Port → Protocol → Questions → Appropriate enumeration
```

not:

```text
Port → random exploit
```

---

## 14. Saving scan results

Good pentesting requires evidence and reproducibility.

### Normal output

```bash
nmap -oN scan.txt TARGET
```

Readable Nmap output.

### Greppable output

```bash
nmap -oG scan.gnmap TARGET
```

Useful for simple text-processing workflows, although XML is generally better for structured machine processing.

### XML

```bash
nmap -oX scan.xml TARGET
```

Structured output that can be parsed by other tools.

### Save major formats together

```bash
nmap -oA initial-scan TARGET
```

This creates multiple output formats with the same basename, making it convenient to preserve evidence from a lab or assessment.

---

## 15. A practical workflow I use in labs

### Step 1 — Check reachability

```bash
nmap -sn TARGET
```

If the authorized target is known to exist but discovery is filtered:

```bash
nmap -Pn TARGET
```

### Step 2 — Initial TCP enumeration

```bash
nmap -sC -sV TARGET
```

### Step 3 — Check all TCP ports

```bash
nmap -p- TARGET
```

If the full scan reveals additional ports, I return to them with service/version detection rather than ignoring them.

### Step 4 — Enumerate by service

Example reasoning:

```text
22/tcp  → SSH enumeration
80/tcp  → Web enumeration
445/tcp → SMB enumeration
8080    → Inspect web service separately
```

### Step 5 — Save evidence

```bash
nmap -sC -sV -oA scans/service-scan TARGET
```

### Step 6 — Research only after enumeration

I research the actual product/version/configuration I observed. I avoid assuming that a search result or matching CVE automatically means the target is vulnerable.

---

## 16. Common mistakes I want to avoid

**Running only one scan.** A default scan can miss services on uncommon ports.

**Treating "host seems down" as proof.** Discovery traffic may be filtered.

**Seeing a version and immediately searching for an exploit.** Enumeration and validation come first.

**Ignoring UDP.** A TCP scan says nothing about UDP exposure.

**Using NSE scripts blindly.** I want to understand what a script checks before running it.

**Not saving results.** Repeating scans wastes time and makes reporting harder.

**Scanning outside scope.** Every scan should be against systems I own or have explicit authorization to test.

---

## 17. Command reference

| Goal | Example |
|---|---|
| Host discovery | `nmap -sn TARGET` |
| Skip discovery | `nmap -Pn TARGET` |
| SYN scan | `sudo nmap -sS TARGET` |
| TCP Connect scan | `nmap -sT TARGET` |
| UDP scan | `sudo nmap -sU TARGET` |
| Specific ports | `nmap -p 22,80,443 TARGET` |
| All TCP ports | `nmap -p- TARGET` |
| Version detection | `nmap -sV TARGET` |
| Default scripts | `nmap -sC TARGET` |
| Show state reason | `nmap --reason TARGET` |
| Fast scan | `nmap -F TARGET` |
| Normal output | `nmap -oN scan.txt TARGET` |
| Greppable output | `nmap -oG scan.gnmap TARGET` |
| XML output | `nmap -oX scan.xml TARGET` |
| Major formats | `nmap -oA scan TARGET` |

---

## What Nmap means to my pentesting methodology

The biggest lesson I have taken from Nmap is that scanning is not about collecting as many flags as possible. It is about reducing uncertainty.

At every stage I ask:

- What do I know?
- What does this response actually prove?
- What information am I still missing?
- Which service deserves deeper enumeration?
- What is the least noisy useful next step?
- Have I saved enough evidence to reproduce my finding?

That mindset turns Nmap from a command-line scanner into part of a repeatable penetration-testing methodology.

---

### Related notes

- [Host Discovery](Host-Discovery.md)
- [NSE and Output](NSE-and-Output.md)
- [Pentesting Workflow](../Methodology/Pentesting-Workflow.md)

> **Lab note:** All examples in this repository are intended for authorized labs, CTFs, and systems where testing permission has been granted.
