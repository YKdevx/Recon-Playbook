# ⚔ Phase 5 — Active Recon (Validation & Attack Surface Probing)

---

## 🎯 Goal

The goal of Active Recon is to **validate and interact with previously discovered attack surfaces** in order to:

* Confirm real endpoints
* Test parameter behavior
* Identify misconfigurations
* Validate threat hypotheses (from Phase 4)

---

## 📥 Input

* High-value targets (Tier 1 & Tier 2)
* `api_endpoints.txt`
* `all_urls.txt`
* `params.txt`
* JS-discovered endpoints
* Threat models (Phase 4 outputs)

---

## 📤 Output

* Verified endpoints
* Confirmed vulnerable surfaces (pre-exploitation)
* Parameter behavior report
* Fuzzing results
* Interaction map of live system

---

## 🧠 Core Idea

> Passive recon shows what *might exist*
> Active recon confirms what *actually reacts*

---

# 🧭 1. Live Target Validation

## 🎯 Objective

Confirm which assets are truly reachable and responsive.

---

## HTTP Probing

```bash id="a1"
httpx -l live_services.txt -status-code -title -tech-detect -follow-redirects > live_verified.txt
```

---

## DNS Resolution

```bash id="a2"
dnsx -l passive_all.txt > resolved.txt
```

---

## Output

* `live_verified.txt`
* `resolved.txt`

---

# 🌐 2. Endpoint Validation

## 🎯 Objective

Check if discovered endpoints are real and interactive.

---

## Test Approach

* Send baseline requests
* Observe status codes
* Identify behavioral differences

---

## Example:

```bash id="a3"
curl -i https://target.com/api/user
```

---

## Observations:

* 200 → active endpoint
* 401 → protected but exists
* 403 → restricted but valid surface
* 404 → potential false positive or hidden route

---

# 🧪 3. Parameter Probing

## 🎯 Objective

Understand how parameters behave under manipulation.

---

## Test Types:

* Missing parameter
* Extra parameter injection
* Invalid datatype
* Boundary values
* Null/empty input

---

## Example:

```bash id="a4"
https://target.com/api/user?id=1
https://target.com/api/user?id=999999
https://target.com/api/user?id=' OR 1=1 --
```

---

## Output:

* `parameter_behavior.txt`

---

# 🔍 4. Fuzzing (Controlled)

## 🎯 Objective

Discover hidden endpoints and parameters.

---

## Directory Fuzzing

```bash id="a5"
ffuf -u https://target.com/FUZZ -w raft-small.txt -mc 200,204,301,302,403
```

---

## API Fuzzing

```bash id="a6"
ffuf -u https://api.target.com/v1/FUZZ -w api-wordlist.txt
```

---

## Parameter Fuzzing

```bash id="a7"
ffuf -u "https://target.com/api/user?FUZZ=test" -w params.txt
```

---

## Output:

* `fuzz_results.txt`

---

# 🧠 5. Behavior Analysis

## 🎯 Objective

Identify how system behaves under different inputs.

---

## Focus Areas:

* Error messages
* Response timing differences
* Status code variations
* Response size changes

---

## Key Insight:

> Behavior change = hidden logic exists

---

# 🧩 6. Authentication Surface Testing

## 🎯 Objective

Understand access control behavior.

---

## Checks:

* Unauthenticated access
* Token reuse
* Role switching
* Cookie manipulation

---

## Hypothesis:

* Can endpoint be accessed without auth?
* Does token restrict only UI, not backend?

---

# 🌐 7. API Interaction Mapping

## 🎯 Objective

Map how API endpoints connect.

---

## What to identify:

* chained endpoints
* dependent requests
* internal API calls
* hidden versions (/v1, /v2, /internal)

---

## Output:

```text id="a8"
api_flow_map.txt
```

---

# 🧠 8. Validated Attack Surface

## 🎯 Objective

Create final refined list of actionable targets.

---

## Output:

* Confirmed endpoints
* High-value parameters
* Fuzzable surfaces
* Weak authentication points

---

```text id="a9"
validated_attack_surface.txt
```

---

# 🧠 Key Insight

> Active Recon is not exploitation.
> It is **system stress testing to reveal hidden logic.**

---

CUSTOM RULE:
- fuzz only validated hypotheses
- no blind fuzzing without signal
- behavior-based testing first

---

# 🚀 Final Transition

After Phase 5:

✔ You have validated attack surface
✔ You understand system behavior
✔ You have tested input boundaries

Next step is no longer recon:

👉 **Exploitation Phase (Bug Hunting / POC Development)**

---
