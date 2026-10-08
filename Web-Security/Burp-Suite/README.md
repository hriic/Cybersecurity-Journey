# Burp Suite — Practical Web Application Testing Notes

**Author:** Yaman | **Track:** Junior Penetration Tester | **Environment:** TryHackMe and authorized labs

> This is a learning journal, not a claim of professional engagements or confirmed vulnerabilities. Examples use fictional lab endpoints. Only test assets within explicit authorization.

## Navigation
1. [HTTP and interception](01-HTTP-and-Proxy.md)
2. [Repeater and request analysis](02-Repeater.md)
3. [Intruder and payload strategies](03-Intruder.md)
4. [Sessions, macros, rules and scope](04-Sessions-Macros-Scope.md)
5. [Practical workflow and troubleshooting](05-Workflow-and-Troubleshooting.md)

## Mental model
**Browser → Burp Proxy → HTTP request inspection → Repeater for controlled experiments → Intruder for structured input testing → evidence and reporting.**

Burp is an interception and testing platform, not an automatic proof of exploitation. A different response length or status is a **lead**; confirm behavior through repeatable tests, context and response bodies.

## Skills covered in my learning
- Navigating Proxy, HTTP history, Repeater and Intruder.
- Identifying request methods, URL paths, parameters, headers, cookies and bodies.
- Sending intercepted requests to Repeater, modifying one variable at a time and interpreting responses.
- Configuring Intruder attack types, payload positions and payload sets.
- Understanding Sniper, Battering ram, Pitchfork and Cluster bomb.
- Examining response lengths, status codes and anomalies rather than blindly trusting one indicator.
- Learning the purpose of session-handling rules, macros and scope.

## Evidence policy
Screenshots and lab flags are **not included** unless captured and verified. Add redacted screenshots under `assets/` when available. Never commit credentials, active session cookies, authorization headers, private target URLs or API keys.

## Quick reference
| Feature | Primary purpose | Key question |
|---|---|---|
| Proxy | Intercept and observe traffic | What did the browser actually send? |
| Repeater | Manually resend modified requests | Does changing this input alter server behavior? |
| Intruder | Systematic payload testing | Which inputs lead to distinct responses? |
| Inspector | Structured request/response view | Where is this parameter or cookie located? |
| Scope | Limit relevant targets | Is this endpoint authorized and relevant? |
| Session handling | Maintain valid state | Is a stale token invalidating the experiment? |

**Next steps:** Practice writing reproducible lab notes with redacted request/response pairs, then study access control, authentication, CSRF, XSS and SQL injection validation in isolated training environments.
