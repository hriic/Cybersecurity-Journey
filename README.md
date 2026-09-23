# Cybersecurity Journey

A practical record of my cybersecurity learning, labs and technical notes. My main direction is **penetration testing and Red Teaming**, with enough defensive knowledge to understand what my activity looks like from the other side.

This repository is not intended to be a badge list or a collection of copied commands. I use it to document **how technologies work, why I choose a technique, what output means, and how one finding changes the next step**.

## Knowledge Base

| Area | What is documented |
| --- | --- |
| [Networking](Networking/) | TCP/UDP, addressing, common protocols, DNS and traffic analysis |
| [Nmap](Nmap/) | Host discovery, scanning, service detection, NSE and output interpretation |
| [Linux](Linux/) | Filesystem, permissions, processes, networking, shell workflow and kernel basics |
| [Windows](Windows/) | Windows/PowerShell fundamentals and Windows Server lab concepts |
| [Active Directory](Active-Directory/) | Domains, DCs, identity, groups, authentication, GPO and permissions |
| [Web Security](Web-Security/) | HTTP, enumeration, curl, Gobuster, Burp, fingerprinting, WebDAV and SQLi concepts |
| [Security Tools](Security-Tools/) | Detailed notes on tools I have actually used and how I choose between them |
| [Methodology](Methodology/) | My evidence-driven penetration-testing workflow |
| [Security Fundamentals](Security-Fundamentals/) | Attack/defense concepts and testing frameworks |
| [TryHackMe](TryHackMe/) | Hands-on topics, lab practice and lessons |
| [CTF Writeups](CTF-Writeups/) | Authorized challenge notes and lessons learned |
| [Blue Team / SOC](Blue-Team-SOC/) | Events, alerts, logs, detections and my SentinelX project |
| [Programming](Programming/) | C++, Python, algorithms and problem solving |

## How I approach a target

My current mental model is:

**scope → reconnaissance → discovery → ports → services → deeper enumeration → hypothesis → authorized validation → local enumeration → privilege escalation → evidence/reporting**

I try to avoid memorizing a chain of commands. For each action I ask:

1. What question am I trying to answer?
2. Why is this tool/protocol appropriate?
3. What does the output actually prove?
4. What does it *not* prove?
5. What should I investigate next?

## Hands-on environment

My practice includes TryHackMe and isolated virtual labs with Linux, Windows and Windows Server. I have worked with network protocols, Nmap, packet analysis, web enumeration, Windows/AD fundamentals and both offensive and defensive security concepts.

## Current direction

My priority is building strong junior penetration-testing fundamentals and then progressing deeper into web testing, Active Directory security and Red Team skills. I also maintain defensive/SOC knowledge because understanding logs and detection makes offensive work more complete.

## Documentation standard

I want each topic in this repository to become something I could explain in an interview:
- concept and purpose;
- how it works;
- commands/tools I have personally practiced;
- interpretation of output;
- security relevance;
- common mistakes or limitations;
- connection to the wider testing methodology.

The repository will continue to grow as I gain hands-on experience.

> All security testing documented here is limited to systems I own or explicitly authorized training environments.