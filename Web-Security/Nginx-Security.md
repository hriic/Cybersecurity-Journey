# Nginx — Architecture, Enumeration and Misconfiguration Notes

## What Nginx is

Nginx is commonly used as a **web server**, **reverse proxy**, **load balancer**, and gateway in front of application servers.

A common deployment:

```text
Client
  |
  v
Nginx :80/:443
  |
  +----> static files
  |
  +----> Node/Express :3000
  |
  +----> Python/Django :8000
```

This distinction matters in pentesting because the HTTP response may pass through Nginx even though the vulnerable application is a different backend.

## Reverse proxy concept

A reverse proxy accepts a request from the client and forwards it to an internal service. This can hide internal ports and centralize TLS, routing, caching, and access controls.

Security questions:

- Which paths are proxied?
- Which paths are served directly?
- Are internal locations accidentally exposed?
- Are headers rewritten safely?
- Are access controls consistent between Nginx and the backend?

## Fingerprinting

Useful observations include:

```bash
curl -I http://TARGET/
```

Look at:

- `Server` header;
- status code;
- redirects;
- default/error pages;
- differences between paths;
- response headers.

Version disclosure helps research, but a banner alone is not proof of exploitability.

## Directory listing

If autoindex/directory listing is enabled, requesting a directory may return a list of files instead of an index page.

Potential exposure includes:

- backups;
- logs;
- source archives;
- configuration copies;
- uploaded files;
- temporary files.

Directory listing is primarily an information-exposure/configuration problem; impact depends on what is listed.

## Backup and forgotten files

During enumeration I do not only search for "admin." Files such as backups and old configurations can reveal more than the live page.

Examples of patterns worth understanding in authorized labs:

```text
.bak
.old
.zip
.tar.gz
~
backup/
config/
```

The lesson is that deployment hygiene is part of security.

## Nginx vs application behavior

If a response says `Server: nginx`, I should not conclude that Nginx generated the application.

I compare:

- Nginx-generated errors vs application-generated errors;
- static vs proxied paths;
- headers added by upstream frameworks;
- different behavior on different locations.

This can reveal multiple layers in the stack.

## Logs

Nginx access logs are valuable defensively. They can record source IP, method, URI, status code, user agent, and timing. Patterns such as repeated 404s, unusual methods, traversal strings, or bursts of enumeration can become detection signals.

## Security lessons

The Nginx topics I practiced reinforced these principles:

1. Identify the layer producing the response.
2. Enumerate content, not only ports.
3. Treat directory indexes and backup files as evidence.
4. Do not equate a version banner with a vulnerability.
5. Understand proxy boundaries before testing the backend.
6. Remember that offensive requests leave useful web-server telemetry.