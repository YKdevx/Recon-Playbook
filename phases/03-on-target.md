# 🎯 Phase 3 — On Target Recon (Deep Attack Surface Mapping)

---

## 🎯 Goal

The goal of this phase is to **understand how the target application actually works** by analyzing:

* Application architecture
* Entry points
* APIs and endpoints
* JavaScript logic
* User-controlled inputs
* Hidden functionality

This phase bridges the gap between recon and vulnerability discovery.

---

## 📥 Input

* Tier 1 & Tier 2 targets from Phase 2
* `all_urls.txt`
* `live_services.txt`
* JavaScript files
* API endpoints
* Parameter lists

---

## 📤 Output

* High-value endpoints
* API surface map
* Parameter inventory
* JS-discovered secrets/endpoints
* Attack surface model per application

---

## 🧠 Core Idea

> If Phase 1 is “finding assets”
> and Phase 2 is “prioritization”
> then Phase 3 is “understanding behavior”

---

# 🧭 1. Application Surface Mapping

## 🎯 Objective

Understand how the application is structured and how data flows.

---

## 🧩 Architecture Identification

Identify application type:

* Monolithic web app
* Server-rendered + legacy backend
* SPA (React / Vue / Angular)
* SPA + REST API
* SPA + GraphQL
* WebSocket-based system

---

## 🛠 Tools

* Wappalyzer
* Built-in DevTools
* Response analysis

---

## 📌 Output

```text id="arc1"
architecture-map.txt
```

---

# 🔑 2. Entry Point Discovery

## 🎯 Objective

Identify all user-controlled inputs.

---

## Types of Entry Points

* Query parameters
* Body parameters
* HTTP headers
* Cookies
* JSON fields
* WebSocket messages

---

## 🧠 Key Rule

> Every input in the application is a potential attack surface.

---

## Output

```text id="ep1"
entry-points.txt
```

---

# 🌐 3. URL & Endpoint Intelligence

## 🎯 Objective

Transform raw URLs into structured attack surfaces.

---

## 3.1 URL Mining

```bash id="url1"
waybackurls target.com > wayback.txt
katana -u https://target.com -d 5 > katana.txt
```

---

## 3.2 Merge & Normalize

```bash id="url2"
cat wayback.txt katana.txt | uro | sort -u > all_urls.txt
```

---

## 3.3 Endpoint Pattern Extraction

```bash id="url3"
grep -Eo "/api/[a-zA-Z0-9/_-]+" all_urls.txt | sort -u > api_endpoints.txt
```

---

## 📌 Output

* `all_urls.txt`
* `api_endpoints.txt`
* `endpoints_map.txt`

---

# 🧠 4. JavaScript Recon (Critical Layer)

## 🎯 Objective

Extract hidden logic, endpoints, and secrets from frontend code.

---

## 4.1 JS Collection

```bash id="js1"
grep "\.js" all_urls.txt > js_urls.txt
```

---

## 4.2 Download & Analyze

```bash id="js2"
mkdir js_files
cat js_urls.txt | while read url; do
  wget -q "$url" -P js_files/
done
```

---

## 4.3 Secret Discovery

Search patterns:

* API keys
* tokens
* internal endpoints
* debug routes

```bash id="js3"
grep -RniE "api|key|token|secret|admin|internal" js_files/
```

---

## 📌 Output

```text id="jsout"
js_secrets.txt
js_endpoints.txt
```

---

# 🔍 5. Parameter Discovery

## 🎯 Objective

Identify all potential input vectors.

---

## Sources

* URLs (`?id=123`)
* JS files
* Forms
* APIs
* Historical data

---

## Extraction

```bash id="p1"
cat all_urls.txt | unfurl keys | sort -u > params.txt
```

---

## 📌 Output

* `params.txt`
* `custom_params.txt`

---

# 🧪 6. Threat Surface Understanding

## 🎯 Objective

Connect everything together into a mental model.

---

## Questions to Ask

* Where does user input go?
* What does the backend trust?
* Where is authentication enforced?
* Are there hidden roles or flows?
* Can endpoints be accessed directly?

---

## 📌 Output

```text id="th1"
attack_surface_map.md
```

---

# 🧠 Key Insight

> Recon is not about collecting endpoints.
> It is about understanding **how the system can be broken.**

---

# 🚀 Next Phase

After Phase 3, we have:

* Full application map
* Entry points
* API surface
* JS intelligence
* Parameter inventory

Next step:

👉 **Phase 4 — Threat Modeling & Vulnerability Hypothesis**

This is where recon starts turning into actual bug discovery.
