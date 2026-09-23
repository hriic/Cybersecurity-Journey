# Nmap Host Discovery — Detailed Notes

Host discovery answers the first reconnaissance question: **which systems appear reachable before I spend time enumerating their ports and services?**

## Discovery vs port scanning

These are separate concepts.

```text
Host discovery → Is there evidence the machine is reachable?
Port scanning  → Which network ports respond, and how?
```

A device may ignore one discovery probe while still exposing services. Firewalls, ACLs and host configuration can all change what I observe.

## Discovery without a port scan

```bash
nmap -sn 10.10.10.0/24
```

`-sn` performs host discovery without proceeding to the normal port scan.

## ICMP discovery

```bash
nmap -PE TARGET
nmap -PP TARGET
nmap -PM TARGET
```

- `-PE` — ICMP Echo Request.
- `-PP` — ICMP Timestamp Request.
- `-PM` — ICMP Address Mask Request.

I do not interpret silence as definitive proof that a host is offline. ICMP may simply be filtered.

## TCP SYN discovery

```bash
nmap -PS80 TARGET
nmap -PS22,80,443 TARGET
```

`-PS` sends TCP SYN probes to the selected ports. TCP-based discovery can reveal systems that do not respond to ICMP probes.

## TCP ACK discovery

```bash
nmap -PA80 TARGET
```

`-PA` uses TCP ACK probes. Different network filtering rules can produce different visibility depending on the probe.

## Treat a known target as online

```bash
nmap -Pn TARGET
```

`-Pn` skips the normal discovery decision and tells Nmap to scan the target as if it is online. In labs this is useful when the assigned target is known to exist but Nmap's discovery probes are filtered.

## Reasoning example

If I receive:

```text
Note: Host seems down.
```

I do not immediately conclude the machine is unavailable. My thought process is:

```text
Is the IP correct?
      ↓
Is my VPN/network route working?
      ↓
Could discovery probes be filtered?
      ↓
Try an appropriate alternative probe / -Pn
      ↓
Continue authorized enumeration
```

This prevents me from confusing **no response to a specific probe** with **proof that no host exists**.

## Key lesson

Host discovery is evidence gathering. Different probes answer the same high-level question in different ways, and network controls can affect the answer.

> Use these techniques only on networks and systems you are authorized to test.
