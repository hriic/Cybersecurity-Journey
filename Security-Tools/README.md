# Security Tools — Practical Notes

This section documents tools I have actually studied or used in authorized labs. The goal is not to build a giant tool list. For every tool I want to know **what problem it solves, what its output means, when I would use it, and what I should verify manually**.

## Nmap

**Purpose:** host discovery, port scanning, service/version detection and NSE-assisted enumeration.

Examples I have practiced:
```bash
nmap -sC -sV TARGET
nmap -p- TARGET
nmap -PE TARGET
nmap -oX scan.xml TARGET
```
I use Nmap to create the initial attack-surface map. A discovered port becomes a question: what service is listening, what version/configuration is visible, and what enumeration makes sense next? I keep deeper notes in [Nmap](../Nmap/).

## Wireshark

**Purpose:** graphical packet capture and protocol analysis.

I use Wireshark to inspect conversations packet by packet, follow protocol behavior and apply display filters. Working with Telnet and mail protocols made the difference between plaintext and encrypted protocols very concrete. A filter such as `imap` can narrow a capture to the protocol I am investigating.

## tcpdump

**Purpose:** lightweight command-line packet capture.

tcpdump is useful when I do not need a GUI or when I am working directly in a terminal. It also helped me learn that capture filters decide what traffic is recorded, while Wireshark display filters decide what already-recorded traffic is shown.

## curl

**Purpose:** make and inspect HTTP requests directly from the terminal.

Example:
```bash
curl -A "R" -L http://TARGET/
```
`-A` sets the User-Agent and `-L` follows redirects. I use curl for quick header/content checks, testing request behavior and understanding a web application without relying only on a browser.

## Gobuster

**Purpose:** content and virtual-host discovery.

I use Gobuster when enumeration suggests that useful resources may not be directly linked. Results still need interpretation: a status code, redirect or repeated response does not automatically mean I found something valuable. Wordlist choice and the correct mode matter.

## Burp Suite

**Purpose:** intercept, inspect and modify web requests in authorized labs.

Burp helps me see the exact HTTP request/response flow behind a browser action. I use it to understand parameters, cookies, headers, authentication flows and application behavior. My priority is understanding the request before modifying it.

## Wappalyzer

**Purpose:** technology fingerprinting.

Wappalyzer provides clues about frameworks, CMSs, libraries and other technologies a site appears to use. I treat fingerprinting as evidence to guide enumeration rather than proof that a particular vulnerability exists.

## Wayback Machine

**Purpose:** historical web reconnaissance.

Archived versions of public pages can reveal old paths, previous application structure or content that is no longer linked. Historical information still needs to be validated against the current authorized target.

## John the Ripper

**Purpose:** password/hash auditing in training environments.

My focus is understanding hash identification, wordlist-based cracking concepts and why strong password storage matters. A hash is not encryption, and the cracking approach depends on the algorithm and available evidence.

## SQLMap

**Purpose:** automate SQL-injection testing after there is a justified injection hypothesis.

I do not want SQLMap to replace understanding SQL injection. Before automation, I should know which input is being tested, how the request reaches the application and what behavior suggests an injection issue.

## Metasploit Framework

**Purpose:** modular exploitation and post-exploitation framework used in controlled labs.

I have studied its basic workflow: identify a relevant module, understand its requirements/options, configure the authorized target and interpret the result. A matching module name or version is not proof that a target is vulnerable.

## Git and GitHub

**Purpose:** version control and technical documentation.

I use GitHub to turn my learning into reviewable evidence: structured notes, project documentation and lab lessons. Commits also let me improve documentation over time instead of treating notes as disposable.

## VMware

**Purpose:** isolated virtual lab environments.

I use VMs for Kali, Windows, Windows Server and intentionally vulnerable/training systems. NAT and host-only networking help me separate lab networking from normal usage. Isolation and scope are part of safe security practice.

## Tool-selection principle

I try to avoid **tool-first thinking**. The better sequence is: understand the question → choose the protocol/technique → choose the tool → interpret output → verify → document the evidence.