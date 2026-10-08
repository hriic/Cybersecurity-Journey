# 01 — HTTP and Burp Proxy

## Purpose
Understand how HTTP requests and responses appear in Burp and how to inspect authorized lab traffic.

## HTTP anatomy
A request contains a method, path, headers and sometimes a body. A response contains a status, headers and response body. Cookies commonly maintain sessions and should be redacted in shared evidence.

## Proxy workflow
1. Open Burp's dedicated browser for a training application.
2. Use Proxy Intercept to pause and inspect a request.
3. Forward the request to continue, or drop it to cancel.
4. Review Proxy HTTP history to identify the relevant endpoint.
5. Send the request to Repeater for one-variable-at-a-time testing.

## Inspector
Inspector presents structured views of query parameters, body parameters, headers and cookies. For a form POST, check the body and Content-Type before editing values.

## HTTPS
The testing browser may need to trust Burp's certificate authority for HTTPS interception. Use an isolated browser profile rather than changing system-wide trust settings.

## What to record
- Method and endpoint.
- Input location and expected value.
- Baseline status and meaningful response content.
- Whether authentication or rotating tokens affect the result.
- A redacted screenshot when available.

## Common mistakes
- Leaving Intercept on and thinking the site is frozen.
- Editing a request for a static asset rather than the intended application action.
- Mistaking a successful HTTP status for successful authorization.
- Publishing live session tokens in screenshots.

## Learning takeaway
Proxy reveals what the client actually sent; Repeater is better for testing a precise hypothesis. The response must be interpreted in context.
