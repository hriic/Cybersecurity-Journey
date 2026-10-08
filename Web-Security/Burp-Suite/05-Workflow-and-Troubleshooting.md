# 05 — End-to-End Lab Workflow and Evidence

## Goal
Use Burp to investigate a web application's behavior systematically rather than guessing payloads or chasing flags without understanding the HTTP exchange.

## Repeatable procedure
### 1. Establish scope
Document the authorized lab host, relevant endpoints, test account and restrictions. Do not include passwords or live cookies in the repository.

### 2. Browse normally
Use Burp's browser and Proxy HTTP history to observe a complete legitimate workflow. Note which request actually changes server state.

### 3. Identify input surfaces
Inspect query parameters, URL path segments, form bodies, JSON properties, cookies and headers. Use Inspector to avoid editing the wrong part of a request.

### 4. Create a baseline
Send the exact request to Repeater. Record method, path, response status and meaningful body text.

### 5. Form a hypothesis
Example: “Changing the product identifier may cause the application to return a different record.” A hypothesis is not a finding.

### 6. Make one controlled change
Modify one value in Repeater. Compare results and account for redirects, cache, authentication and token expiration.

### 7. Scale only when justified
Use Intruder with a small, scoped list if systematic comparison is necessary. Choose Sniper for independent parameter testing, Pitchfork for paired lists and Cluster bomb only when combinations are necessary and the request volume is acceptable.

### 8. Triage anomalies
Filter or sort results by status, length and relevant response content. A response-size difference is a clue, not proof. Reproduce promising results in Repeater.

### 9. Manage session state
If requests become invalid, check cookies, redirects and CSRF state before assuming the tested input caused the error. Add session rules or macros only if needed.

### 10. Report responsibly
Record observations, reproduction steps, evidence, impact **if verified**, limitations and remediation suggestions. For lab flags, record the reasoning rather than publishing secrets from a real system.

## Example sanitized observation
```text
Lab: Authorized training application
Tool: Burp Repeater
Endpoint: GET /products/{id}
Baseline: /products/1 → 200, product A
Change:   /products/2 → 200, product B
Finding:  Identifier controls selected public product
Security impact: None established without an access-control boundary
Next test: Compare permissions using distinct authorized lab accounts
```

## Frequently encountered blockers
| Symptom | Check first | Next step |
|---|---|---|
| Browser appears frozen | Proxy Intercept may be on | Forward/drop or disable interception |
| Repeater gives login page | Cookie expired | Reauthenticate in lab; inspect session state |
| POST change ignored | Wrong field or encoding | Inspect body parameters and Content-Type |
| Intruder outputs same length | Generic error/login response | Open response bodies and verify payload insertion |
| Intruder request count huge | Cluster bomb combinations | Reduce sets or use a more selective attack type |
| Macro not used | Rule does not match | Check tool/URL scope and configured action |
| Flag not found | Wrong hypothesis or hidden condition | Return to task wording, baseline and response analysis |

## Suggested screenshot structure
```text
assets/
  proxy-http-history-redacted.png
  repeater-baseline-redacted.png
  intruder-positions-redacted.png
  intruder-results-redacted.png
  session-rule-redacted.png
```
These are **suggested filenames**, not screenshots currently committed.

## Future study plan
- Authentication and session management.
- Access control / IDOR with two lab identities.
- CSRF validation and token lifecycle.
- XSS input contexts and output encoding.
- SQL injection fundamentals in purpose-built labs.
- Reporting with reproducible, minimal-impact evidence.

## Reflection
The important improvement is moving from “try values until a flag appears” to **observe → hypothesize → test → verify → document**. This is the habit that makes tool knowledge transferable to real penetration-testing work.
