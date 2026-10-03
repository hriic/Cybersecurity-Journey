# Web Attacks 1 & 2 — Practical Methodology

## Purpose

This file consolidates lessons from authorized web-server and application training. I intentionally document reusable reasoning rather than room flags or copied walkthrough answers.

## Start with architecture

Before investigating a security issue I determine what is serving the application: Apache, Nginx, IIS, Node.js/Express, a Python service, Next.js, Django, or a reverse proxy in front of another backend.

The technology changes which files, methods, error messages, modules, and configuration mistakes are relevant.

## Establish a baseline

I record normal HTTP behavior first: status codes, server headers, redirects, cookies, content types, security headers, and visible application behavior.

Without a baseline it is difficult to know whether a modified request actually changed anything.

## HTTP methods

Methods such as GET, POST, OPTIONS, PUT, DELETE, and WebDAV-specific methods have different purposes. An allowed method is evidence of capability, not automatically a vulnerability.

The security question is whether the capability is necessary, properly authenticated, correctly authorized, and safely configured.

## Content discovery

I learned to investigate directories, files, backups, uploads, debug endpoints, configuration artifacts, robots.txt entries, static JavaScript, and directory indexes.

A discovered path matters because of what it reveals or permits, not because a discovery tool printed it.

## Fingerprinting through errors

Error behavior can disclose framework names, database technology, source paths, route structure, development mode, and server modules.

I compare valid and invalid requests to identify which layer is generating a response.

## Apache lessons

Apache can expose information through banners, directory indexes, forgotten files, modules, and diagnostic endpoints.

I studied /server-status as an example of an administrative/diagnostic feature that should not expose operational information to unauthorized users.

## Python development server

A simple Python HTTP server is useful for labs and temporary file serving, but it should not be confused with a hardened production web stack.

Recognizing development tooling helps explain directory behavior, headers, and limitations.

## Nginx lessons

My Nginx study focused on server fingerprinting, directory indexing, backup/configuration exposure, proxy boundaries, and distinguishing Nginx-generated responses from upstream application responses.

## Node.js / Express lessons

My Express study covered framework indicators, sessions, API route behavior, verbose errors, debug/environment exposure, static configuration, object input, and JavaScript Prototype Pollution concepts.

## IIS / WebDAV lessons

The IIS labs reinforced a chain-based approach:

IIS fingerprint → WebDAV discovery → authentication analysis → HTTP method analysis → information-disclosure clues → permission validation → impact validation.

Each step provides evidence for the next. No single observation should be treated as proof of full compromise.

## URL encoding

HTTP clients encode special characters in URLs and form data. Understanding encoding is important when parameters, filenames, filters, and application parsers interpret input differently.

## Upload vs execution

I learned to separate these questions:

- Can content be uploaded or created?
- Where is it stored?
- Can it be retrieved?
- How does the server interpret its file type?
- What permissions apply to the server process?

This distinction is critical when evaluating upload-related findings.

## Repeatable workflow

Scope → service discovery → HTTP baseline → technology fingerprinting → content/route enumeration → method/authentication analysis → information-disclosure review → specific hypothesis → authorized validation → confirm actual impact → record evidence and remediation.

## Reporting mindset

A useful finding should explain Evidence, Condition, Impact, Required Chain, and Remediation.

That is more valuable than a list of commands because it demonstrates that I understand why the issue exists and how to break the chain.