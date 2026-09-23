# Web Security & Enumeration

Web testing is one of the areas I am developing for penetration testing. I am focusing first on understanding HTTP and application behavior, then using tools to test specific hypotheses.

## HTTP mental model

A browser sends an HTTP request containing a method, path, headers and sometimes a body. The server returns a status code, headers and a response body. Understanding that exchange makes tools such as curl and Burp Suite much easier to reason about.

Common methods I encounter include GET and POST, while methods such as PUT, DELETE or OPTIONS can become relevant depending on application configuration.

## Status codes

I read status codes as clues:
- **2xx**: request succeeded.
- **3xx**: redirection.
- **4xx**: client-side/access/resource conditions such as 401, 403 or 404.
- **5xx**: server-side failure.

A status code by itself is not a vulnerability; I compare content, redirects, authentication behavior and context.

## Initial enumeration

For an authorized web target I may inspect:
- page title and visible functionality;
- response headers and redirects;
- HTML/source comments and linked resources;
- `robots.txt`;
- technologies/frameworks using clues or Wappalyzer;
- historical public pages with the Wayback Machine;
- directories/files and virtual hosts;
- authentication flows and cookies;
- supported HTTP methods.

## curl

curl lets me interact with HTTP directly and see behavior without relying only on a browser.

```bash
curl -I http://TARGET/
curl -A "R" -L http://TARGET/
```

I have practiced changing the User-Agent, following redirects and searching returned content for technology/version clues.

## Gobuster

Gobuster can perform directory/file and virtual-host discovery using wordlists. I have practiced its web-enumeration modes and learned that results need validation: repeated status codes, redirects and wildcard behavior can create misleading output.

## Burp Suite

Burp Suite lets me intercept and inspect the exact request behind a browser action. This is useful for understanding parameters, headers, cookies, sessions and login behavior before I modify anything.

## Technology fingerprinting

Tools such as Wappalyzer and response/source clues can suggest what technologies are running. I use that information to narrow research, but I do not assume a product is vulnerable just because a version string exists.

## WebDAV

I have encountered WebDAV in labs. Because WebDAV extends HTTP with content-management capabilities, exposed methods and authentication/configuration deserve careful inspection. I treat a 401 response as evidence that a resource exists and requires authentication, not as an automatic vulnerability.

## SQL injection and SQLMap

I have studied SQL injection concepts and SQLMap in training environments. My preferred order is **understand input → observe request → form an injection hypothesis → test carefully → use automation where justified**. SQLMap should accelerate validated testing, not replace understanding.

## What I am improving

I am building toward stronger manual web testing, OWASP-style methodology, authentication/session analysis and the ability to explain *why* each request is being made rather than following a walkthrough blindly.