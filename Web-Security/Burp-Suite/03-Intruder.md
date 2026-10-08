# 03 — Intruder: Attack Types, Positions and Payloads

## What Intruder does
Intruder automates a controlled series of HTTP requests using selected insertion points and payload sets. It is useful for input discovery and differential response analysis in authorized labs. Request counts, concurrency and availability depend on configuration and Burp edition.

## Main configuration
- **Positions / attack type:** Choose where payloads are inserted and how combinations are generated.
- **Payloads:** Supply lists, sequences or other generated values.
- **Payload processing:** Transform values before insertion.
- **Resource pool / settings:** Control request behavior and load, where supported.
- **Results:** Review status, response length, timing and extracted data.

### Payload positions
The symbol **`§`** marks the beginning and end of a payload position in Burp's attack editor. Example:
```http
GET /products/§1§ HTTP/1.1
Host: lab.example
```
The selected value is replaced for each generated request. Do not assume the marker itself is transmitted to the server.

## Four attack types
| Attack type | How positions receive values | Request count |
|---|---|---|
| **Sniper** | One position changes at a time; others stay baseline | For P positions and N payloads: P × N (when each position uses that list) |
| **Battering ram** | Same payload is inserted into every selected position | N |
| **Pitchfork** | Corresponding entries from separate sets are paired | Length of shortest set (typically) |
| **Cluster bomb** | Every combination across independent sets | Product of set sizes |

### Example with two positions
Set A = `[alpha, beta]`, set B = `[1, 2, 3]`.

**Pitchfork** pairs `(alpha,1)`, `(beta,2)` and stops at the shorter list: **2 requests**.

**Cluster bomb** produces `(alpha,1)`, `(alpha,2)`, `(alpha,3)`, `(beta,1)`, `(beta,2)`, `(beta,3)`: **6 requests**.

**Battering ram** repeats one selected payload across both positions.

**Sniper** probes each position separately, useful when identifying which input matters.

> Counts above assume no retries, extra baseline requests or additional processing behavior. Inspect Burp's estimated request count before launching.

## Payload sets and processing
A **payload set** supplies values to one logical payload stream. For Cluster bomb, each position can be associated with its own set. Select the correct set before configuring its list.

**Payload processing** can prepend, append, encode or transform values. To add characters to the **end** of each payload, use an **Add suffix** processing rule (exact menu wording may vary by version). Preview the resulting values before sending.

Example:
```text
Input:     admin
Suffix:    -test
Result:    admin-test
```

## Results interpretation
A request with an unusual response length is only a candidate. Open the full response and ask:
1. Is it a login page, error page or genuine target content?
2. Does it differ from the baseline after removing dynamic content?
3. Is the result repeatable in Repeater?
4. Was the session valid and the target in scope?

If all responses show the same length (for example, 680 bytes), do **not** assume the attack worked or failed. Check the request positions, session validity, redirects, response bodies, and whether the payloads were actually inserted.

## Safe lab workflow
1. Select a low-impact endpoint within the lab scope.
2. Confirm one valid baseline request in Repeater.
3. Mark only the parameter(s) needed.
4. Choose the attack type that matches the hypothesis.
5. Load small payload sets; preview and calculate expected requests.
6. Set a conservative request rate and launch.
7. Sort by meaningful differences; inspect and reproduce manually.
8. Record conclusions, including negative findings.

## Troubleshooting
- **Cannot select payload set 1:** Check the attack type and number of positions. A single-set attack may automatically show only its one set; numbering and interface differ by version.
- **All lengths identical:** Inspect bodies, redirections, cookies and request placement.
- **Unexpected request count:** Recalculate Sniper versus Cartesian-product behavior.
- **Payload not reaching server:** Verify marker placement, encoding and processing preview.
- **Many errors or 429s:** Stop and reduce load; respect lab limits.

## Ethics
Do not use Intruder against third-party authentication services or production accounts without explicit written authorization and a safe rate limit.
