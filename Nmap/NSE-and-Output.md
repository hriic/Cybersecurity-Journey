# Nmap NSE & Output — Detailed Notes

This note covers two parts of my Nmap workflow: using the **Nmap Scripting Engine (NSE)** for targeted enumeration and preserving scan results for later analysis and reporting.

## What is NSE?

NSE extends Nmap with Lua scripts that can perform service discovery, enumeration and other security-related checks.

In Kali/AttackBox-style environments, scripts are commonly stored in:

```text
/usr/share/nmap/scripts/
```

The important lesson for me is to select scripts because they answer a specific enumeration question—not simply because a script exists.

## Default scripts

```bash
nmap -sC TARGET
```

`-sC` runs Nmap's default script selection.

I commonly combine it with version detection during authorized labs:

```bash
nmap -sC -sV TARGET
```

This gives me service/version information plus additional context from relevant default scripts.

## Running a specific script

```bash
nmap --script SCRIPT_NAME TARGET
```

If the script is relevant only to a particular service, I can scope the port as well:

```bash
nmap -p PORT --script SCRIPT_NAME TARGET
```

## Understanding a script before using it

```bash
nmap --script-help SCRIPT_NAME
```

I use script help to understand what the script does, which protocol it targets and what arguments it accepts.

I can also inspect/search the local script collection:

```bash
ls /usr/share/nmap/scripts/
grep -R "keyword" /usr/share/nmap/scripts/
```

Examples I encountered while training include HTTP and SSH enumeration scripts such as `http-robots.txt` and `ssh2-enum-algos`.

## Script arguments

Some NSE scripts accept parameters:

```bash
nmap --script SCRIPT_NAME --script-args 'key=value' TARGET
```

I check the script documentation instead of guessing argument names.

---

# Saving scan output

Saving evidence is part of my methodology. It lets me compare results, parse them later and build reports without unnecessarily repeating scans.

## Normal output — -oN

```bash
nmap -oN scan.txt TARGET
```

Human-readable Nmap output.

## Greppable output — -oG

```bash
nmap -oG scan.gnmap TARGET
```

Designed for simple text-oriented processing. For modern structured parsing, XML is generally more appropriate.

## XML output — -oX

```bash
nmap -oX scan.xml TARGET
```

XML is structured and useful when scan data will be consumed by software.

## Save major formats — -oA

```bash
nmap -oA scans/initial TARGET
```

This saves major Nmap output formats using the same basename.

A simple lab structure I like is:

```text
target/
└── scans/
    ├── initial.nmap
    ├── initial.gnmap
    └── initial.xml
```

## Why I save results

```text
Scan
 ↓
Save evidence
 ↓
Enumerate services
 ↓
Record findings
 ↓
Reproduce / report
```

This is cleaner than rerunning commands every time I need to remember which ports or services were discovered.

## Rule I follow

**NSE complements manual enumeration; it does not replace it.**

A script result is another piece of evidence. I still validate important findings and understand the underlying service before drawing conclusions.

> All commands in these notes are for authorized labs, CTFs and permitted security assessments.
