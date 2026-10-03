# Web Fingerprinting — Evidence-Based Cheat Sheet

This is a quick reference for observations I have practiced. A clue narrows a hypothesis; it does not automatically prove the technology or a vulnerability.

| Observation | Possible meaning | Next question |
| --- | --- | --- |
| Server: nginx | Nginx is in the HTTP path | Is it serving content or proxying upstream? |
| Server: Microsoft-IIS/10.0 | IIS 10 is exposed | Is ASP.NET, WebDAV, or Windows authentication involved? |
| X-Powered-By: Express | Express likely handled the response | Which routes, middleware, and session controls exist? |
| connect.sid | Express-style session clue | How is authenticated state enforced server-side? |
| Cannot GET /path | Express-like route behavior | Does another HTTP method handle the route? |
| csrfmiddlewaretoken | Django-style CSRF clue | Which state-changing actions require it? |
| window.__next_f | Next.js App Router/RSC artifact | What routing and middleware model is used? |
| 401 Unauthorized | Authentication required | Which authentication schemes are advertised? |
| 201 Created | A resource was created | Is it reachable and how is it handled? |
| Directory index | Listing is enabled | Are backups, configuration, or uploads exposed? |
| Verbose stack trace | Development/error disclosure | What paths, framework, or database details leaked? |
| Response differences involving short-name patterns | Possible IIS naming disclosure | Can a real resource be inferred and independently verified? |

## Interpretation rules

- Banner does not equal vulnerability.
- Open port does not equal exploitable service.
- 401 is authentication evidence, not failed enumeration.
- Write permission does not equal code execution.
- Created content does not equal executable content.
- Server-side execution does not automatically equal administrator/SYSTEM.
- An internal database clue does not mean the database is remotely exposed.
- A framework artifact does not mean the framework is vulnerable.

The goal is to turn each response into a better next question.