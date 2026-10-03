# Web Security Knowledge Base

This directory documents the web-security concepts I have studied and practiced in authorized labs. My goal is to preserve **reasoning, architecture, evidence, and methodology** rather than collect copied commands or room answers.

## Study map

| Topic | What I documented |
| --- | --- |
| [Modern Web Stacks](Modern-Web-Stacks.md) | Frontend/backend architecture, MERN, Next.js, middleware, sessions, debug/config exposure, Prototype Pollution concepts |
| [Express & Node.js](Express-and-NodeJS.md) | Node vs Express, middleware, routes, sessions, static files, verbose errors, database clues |
| [Nginx](Nginx-Security.md) | Web server vs reverse proxy, directory indexing, backup exposure, upstream boundaries, logging |
| [IIS, WebDAV & NTLM](IIS-WebDAV-NTLM.md) | IIS/ASP.NET, WebDAV methods, 401/201 interpretation, NTLM concepts, 8.3 short-name behavior, process context |
| [Django & SQL Injection](Django-and-SQL-Injection.md) | Django clues, CSRF context, input-to-query reasoning, error-based SQLi concepts, SQLMap methodology |
| [Web Attacks Lab Methodology](Web-Attacks-Lab-Methodology.md) | Apache, Python HTTP server, Nginx, Express, IIS/WebDAV and the repeatable testing workflow |
| [HTTP & Enumeration](HTTP-and-Enumeration.md) | Requests/responses, methods, status codes, headers, content discovery and evidence interpretation |
| [Fingerprinting Cheat Sheet](Web-Fingerprinting-Cheatsheet.md) | Fast mapping from observed clue → likely meaning → next question |

## Core mental model

A web application is usually layered:

```text
Browser
   ↓
HTTP / HTTPS
   ↓
Web Server / Reverse Proxy
   ↓
Application / Framework / API
   ↓
Database / Internal Services
```

My first job is to determine which layer produced the behavior I am seeing.

## Evidence before conclusions

I try to keep claims precise:

- a banner identifies a likely technology; it does not prove a vulnerability;
- a 401 proves an authentication boundary exists; it does not prove weak credentials;
- an allowed HTTP method proves capability; it does not prove unsafe authorization;
- file creation does not automatically mean server-side execution;
- application execution does not automatically mean administrative privileges;
- a database error does not prove the database is directly reachable over the network.

## My current web-testing workflow

```text
Scope
  ↓
Service discovery
  ↓
HTTP baseline
  ↓
Technology fingerprinting
  ↓
Content / route / API enumeration
  ↓
Authentication and session analysis
  ↓
Input and trust-boundary analysis
  ↓
Specific vulnerability hypothesis
  ↓
Authorized manual validation
  ↓
Impact verification
  ↓
Evidence + remediation
```

## Tools I have practiced

I use tools such as curl, Nmap/NSE, Gobuster, Burp Suite, Wappalyzer, browser developer tools, and SQLMap where appropriate. The tool is secondary to the question I am trying to answer.

## Documentation rule

For every web topic I want to be able to explain:

1. What is the technology?
2. Where does it sit in the architecture?
3. How can I recognize it?
4. What normal behavior should I expect?
5. Which configuration or coding mistakes matter?
6. What evidence proves the finding?
7. What does the evidence **not** prove?
8. How does the finding connect to the wider attack chain?
9. How should it be remediated or detected?

> All testing described in this repository is limited to systems I own or explicitly authorized training environments.