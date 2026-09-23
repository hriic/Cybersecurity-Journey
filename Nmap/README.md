# Nmap

Nmap is one of the main tools I use during reconnaissance and enumeration.

## Commands I have practiced

```bash
nmap -sC -sV TARGET
nmap -p- TARGET
nmap -PE TARGET
nmap -PP TARGET
nmap -PM TARGET
nmap -oG scan.txt TARGET
nmap -oX scan.xml TARGET
```

`-sC` runs default NSE scripts and `-sV` performs service/version detection. `-p-` checks all TCP ports.

For host discovery I have studied ICMP Echo (`-PE`), Timestamp (`-PP`) and Address Mask (`-PM`).

NSE scripts are normally stored under `/usr/share/nmap/scripts/`.

## My workflow

1. Find live hosts.
2. Identify open ports.
3. Detect services and versions.
4. Review relevant scripts and service information.
5. Enumerate each exposed service.
6. Decide what deserves deeper investigation.

I am trying to understand why I run a scan instead of just memorizing flags.