# 02 — Repeater: Controlled Request Experiments

## Purpose
Repeater allows a tester to edit and resend a single HTTP request, observe responses and compare hypotheses without relying on a browser's UI.

## From Proxy to Repeater
1. Capture or locate the desired request in **Proxy → HTTP history**.
2. Right-click → **Send to Repeater**.
3. Open **Repeater**; the transferred request appears in a request tab/editor.
4. Review the request using raw/pretty views and Inspector where available.
5. Change **one variable**, press **Send**, then inspect status, headers, response body and timing.
6. Restore the baseline or duplicate a tab before trying another hypothesis.

**Lab example**
```http
GET /products/1 HTTP/1.1
Host: lab.example
Cookie: session=REDACTED
```
Change `/products/1` to another permitted lab identifier and compare results. An ID change alone is **not** evidence of IDOR: verify the identity, permissions and actual access to another user's protected resource in the authorized lab.

## Reading results
| Signal | Possible interpretation | Confirmation needed |
|---|---|---|
| 200 | Request processed | Did the body contain expected protected content? |
| 302 | Redirect | Check Location and destination behavior |
| 401 | Authentication missing/invalid | Confirm session and auth mechanism |
| 403 | Request forbidden | Compare authenticated and unauthenticated cases |
| 404 | Resource absent or intentionally hidden | Compare known-valid baseline |
| 500 | Server-side error | Reproduce safely; do not assume exploitability |
| Different length | Different representation | Inspect actual content and dynamic elements |

## Practical habits
- Preserve a known-good baseline request.
- Track the exact field changed and why.
- Distinguish reflected input from executed behavior.
- Compare normalized responses when timestamps or rotating tokens add noise.
- Avoid high-impact actions; use lab-safe requests.

## Mini report template
**Hypothesis:** A parameter may influence server-side resource selection.

**Baseline:** Method/path, expected response and sanitized evidence.

**Change:** Exact parameter change.

**Observed:** Status, meaningful body difference and repeatability.

**Conclusion:** Supported, unsupported or inconclusive.

**Limitations:** Session state, CSRF token rotation, caching or dynamic content.
