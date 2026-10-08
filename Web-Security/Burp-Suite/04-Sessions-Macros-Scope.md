# 04 — Sessions, Macros, Rules and Scope

## Why state matters
A valid request may require cookies, a logged-in session, anti-CSRF tokens or a specific workflow sequence. Replaying a stale request can produce a login redirect or rejection unrelated to the input being tested.

## Session handling rules
A **session handling rule** tells Burp when to apply one or more actions to selected requests. A rule can be limited by tool, URL scope and other criteria depending on Burp version. Actions may update cookies or invoke a macro.

**Conceptual flow:**
```text
Outgoing test request
  → rule matches tool and URL
  → perform session action / run macro
  → update relevant request state
  → send test request
  → inspect response
```

## Macros
A **macro** records a sequence of HTTP requests used to reproduce application state, such as loading a form to obtain a fresh anti-CSRF token before submitting it. Macros are not magical login bypasses: they replay authorized workflow steps using the current session and configured parameter extraction.

### Example: rotating form token
1. Open the form in an authorized training app.
2. Observe that the GET response includes a changing hidden token.
3. Record the required GET request(s) as a macro.
4. Configure a session rule to run the macro before a matching submission.
5. Configure token extraction/update behavior as appropriate to the Burp version.
6. Verify in a controlled request that the outgoing token matches the fresh form token.
7. Compare with a deliberately stale token to understand server behavior.

### Macro not appearing in a rule?
- Confirm the macro was saved in the correct project/configuration context.
- Check whether the session action is configured to **run a macro**.
- Check rule scope and tool selection.
- Verify whether the Burp version uses a different settings layout.
- Reopen the macro editor and confirm recorded items exist.
- Do not assume a recorded macro is automatically attached to Intruder.

## Rules versus macros
| Concept | Responsibility |
|---|---|
| Macro | **What sequence** of requests is replayed |
| Rule | **When, where and for which tool** actions apply |
| Scope | **Which target URLs** are relevant or authorized |
| Cookie jar | Stored cookie state Burp may use for session actions |

## Scope
Define authorized hosts and paths in **Target → Scope** or the equivalent current settings. Scope filtering helps reduce irrelevant HTTP history and prevents accidental attention to third-party assets, but **scope alone is not a legal authorization control or universal network firewall**. Check each tool's targeting and configuration before sending requests.

## Validation checklist
- Is the request genuinely in scope?
- Does the rule apply to the correct Burp tool?
- Does the macro obtain current state?
- Are extracted tokens inserted in the correct place?
- Are redirects masking the real result?
- Does the request still succeed without the payload change?
- Have secrets been redacted from screenshots and documentation?

## Learning note
These notes cover the purpose and troubleshooting of session rules and macros. A fully verified token-refresh configuration and its screenshots should be added only after successful lab reproduction.
