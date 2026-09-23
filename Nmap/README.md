# Nmap — Discovery and Enumeration Notes

Nmap is one of the tools I use most in my penetration-testing labs. I use it to answer progressively more specific questions rather than running flags blindly.

## 1. What hosts are reachable?

Host discovery reduces a network range to systems that appear alive. I have studied ICMP Echo (`-PE`), Timestamp (`-PP`) and Address Mask (`-PM`) discovery as well as TCP-based discovery.

A host that does not answer one probe is not automatically offline. Firewalls and filtering can change what I see.

## 2. What ports are exposed?

A TCP SYN scan (`-sS`) uses SYN behavior to infer port state. A full-port scan with `-p-` checks TCP ports 1–65535 rather than only the common default set.

```bash
nmap -sS TARGET
nmap -p- TARGET
```

I read **open**, **closed**, and **filtered** as different pieces of evidence rather than treating every non-open result the same.

## 3. What is actually listening?

```bash
nmap -sC -sV TARGET
```

`-sV` performs service/version detection. `-sC` runs Nmap's default NSE script set. Together they often provide a useful second pass after basic discovery.

Version strings are leads, not vulnerability verdicts. I still need to understand the product, configuration and exposure.

## 4. Nmap Scripting Engine (NSE)

NSE scripts extend Nmap with discovery, enumeration and vulnerability-related checks. In my AttackBox/Kali-style environments they are stored under:

```text
/usr/share/nmap/scripts/
```

I have worked with HTTP and SSH-related scripts and learned to search script descriptions before using them. Examples I encountered include `http-robots.txt` and `ssh2-enum-algos`.

## 5. Reasons and scan interpretation

The `--reason` option helps explain why Nmap assigned a state. This is useful for learning the relationship between packets received and Nmap's conclusion.

```bash
nmap -sS -F --reason TARGET
```

## 6. Saving evidence

```bash
nmap -oN scan.txt TARGET
nmap -oG scan.gnmap TARGET
nmap -oX scan.xml TARGET
```

Normal output is readable, greppable output is convenient for some text workflows, and XML is structured for tools/parsing. Saving results gives me a baseline and avoids repeating scans unnecessarily.

## My enumeration workflow

1. Confirm scope and authorization.
2. Discover reachable hosts.
3. Identify exposed ports.
4. Detect services and versions.
5. Use relevant NSE checks when they answer a specific question.
6. Enumerate each service with protocol-specific tools.
7. Research evidence rather than guessing vulnerabilities.
8. Save results and document why I chose the next step.

Related notes: [Host Discovery](Host-Discovery.md) · [NSE and Output](NSE-and-Output.md) · [Pentesting Workflow](../Methodology/Pentesting-Workflow.md).