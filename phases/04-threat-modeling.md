# ⚔ Phase 4 — Threat Modeling (Attack Hypothesis Phase)

---

## 🎯 Goal

The goal of this phase is to **convert recon data into vulnerability hypotheses**.

Instead of collecting more information, we now ask:

> “How can this system be broken?”

---

## 📥 Input

* Attack surface map (Phase 3)
* Endpoints & parameters
* JS analysis results
* API structure
* Entry points inventory

---

## 📤 Output

* Vulnerability hypotheses list
* High-risk flows
* Attack scenarios per endpoint
* Testing priorities

---

## 🧠 Core Idea

> Recon shows what exists.
> Threat modeling shows what can go wrong.

---

# 🧭 1. Application Flow Analysis

## 🎯 Objective

Understand how data moves inside the application.

---

## What to map:

* Authentication flow
* Authorization flow
* Data creation flow
* Data update/delete flow
* API consumption flow
* Role-based access flow

---

## Key Question:

> What happens if any step in this flow is bypassed or modified?

---

# 🔐 2. Authentication Threat Modeling

## 🎯 Objective

Identify weaknesses in login/session systems.

---

## Checkpoints:

* Session generation logic
* JWT structure (if exists)
* Cookie handling
* Token expiration
* Session reuse
* Multi-device login behavior

---

## Vulnerability Hypotheses:

* Session fixation possible?
* Token replay possible?
* Weak JWT validation?
* Missing session invalidation?

---

# 🧾 3. Authorization (IDOR Thinking)

## 🎯 Objective

Find broken access control paths.

---

## What to analyze:

* User ID parameters
* Object references (id, uuid)
* API resource access patterns
* Role separation (user/admin/staff)

---

## Hypothesis Examples:

* Can user A access user B’s data?
* Can low-privilege role call admin endpoints?
* Are object IDs predictable?

---

# 🌐 4. API Threat Modeling

## 🎯 Objective

Analyze API trust boundaries.

---

## Key Checks:

* Does API trust client-side input?
* Are endpoints directly callable?
* Is there missing rate limiting?
* Are internal APIs exposed?

---

## Hypotheses:

* API allows unauthorized object manipulation?
* Missing auth on internal endpoints?
* Verb tampering possible?

---

# 📦 5. Input Attack Surface Mapping

## 🎯 Objective

Analyze all user-controlled inputs.

---

## Input Types:

* Query parameters
* JSON body fields
* Headers
* Cookies
* WebSocket messages

---

## Hypotheses:

* Input reflected in response → XSS risk
* Input used in DB queries → SQLi risk
* Input used in file paths → LFI/RFI risk

---

# 🧠 6. JavaScript-Based Threat Modeling

## 🎯 Objective

Use frontend logic to infer backend weaknesses.

---

## What to look for:

* Hidden endpoints
* Debug functions
* Feature flags
* Internal APIs
* Unused parameters

---

## Hypotheses:

* Hidden endpoint accessible without auth?
* Internal API exposed in frontend?
* Disabled features still reachable?

---

# 🧪 7. Attack Scenario Construction

## 🎯 Objective

Turn hypotheses into testable scenarios.

---

## Example Format:

```text id="atk1"
Scenario: IDOR on /api/user/{id}
Hypothesis: User ID is not validated properly
Test: Change ID in request
Expected: Access other users' data
```

---

# 📊 8. Prioritization of Threats

## Scoring Factors:

* Impact (data exposure / account takeover)
* Exploitability (low friction vs complex)
* Exposure (public vs internal)
* Authentication requirement

---

## Output:

```text id="sc1"
high_risk_threats.txt
medium_risk_threats.txt
low_risk_threats.txt
```

---

# 🧠 Key Insight

> Recon collects endpoints.
> Threat modeling converts them into attack plans.

---

# 🚀 Next Phase

After Phase 4, you are no longer “collecting information”.

You are now:

👉 **Executing structured vulnerability discovery**

Next step:

## ⚔ Phase 5 — Active Recon (Validation & Exploitation Surface Testing)

Where we:

* test hypotheses
* validate endpoints
* fuzz parameters
* confirm real vulnerabilities
