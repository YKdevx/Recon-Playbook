# 🔍 Phase 1 — Passive Recon (Wide Recon)

## 🎯 Goal

The goal of Passive Recon is to **expand the attack surface without interacting directly with the target infrastructure**.

This phase focuses on discovering:

* Root domains
* Subdomains
* External assets
* Historical endpoints
* Hidden or forgotten services

---

## 📥 Input

* Target scope (domain or program)
* Root domain(s)
* Publicly available intelligence sources

Example:

```
example.com
```

---

## 📤 Output

* Validated root domains
* Subdomain list
* External asset inventory
* Historical URLs
* Initial attack surface map

---

## 🧠 Core Idea

Passive Recon is about **collecting fragmented public data and merging it into a structured asset map**.

The main principle:

> Small signals → aggregated → meaningful attack surface

---

# 🌐 1. Wide Recon (Attack Surface Expansion)

## 🎯 Objective

Increase visibility of all possible assets related to the target.

---

## 🔎 1.1 Search Engine Dorking

### Purpose

Find indexed assets, hidden subdomains, and leaked structures.

### Examples:

```
intitle:example
intext:example
intext:"2026 Example. All rights reserved"
site:example.com -www
```

### Output:

```
COMPANY-roots.txt
```

---

## 🕵️ 1.2 Reverse WHOIS / Ownership Mapping

### Purpose

Discover related domains via registrant data.

### Tools:

* website.informer
* SecurityTrails (optional)
* Whois history databases

### Output:

```
related-domains.txt
```

---

## 🌍 1.3 TLD Expansion Analysis

### Purpose

Identify forgotten or misconfigured domains across TLD space.

### Approach:

* Enumerate TLDs
* Brute-check DNS resolution
* Validate active assets

### Output:

```
tld-scan-results.txt
```

---

## 🧱 1.4 CIDR to Host Discovery *(Only in Scope)*

### Purpose

Discover hidden hosts inside known IP ranges.

### Tools:

```
prips → hakip2host → dnsx
```

### Output:

```
cidr-hosts.txt
```

---

# 🌐 2. Passive Subdomain Enumeration

## 🎯 Objective

Collect subdomains from passive sources without interacting with target infrastructure.

---

## Tools

* subfinder
* amass (passive mode)
* findomain

---

## Execution

```
subfinder -d target.com -all -silent -o subfinder.txt
findomain -t target.com > findomain.txt
amass enum -passive -d target.com > amass.txt
```

---

## Certificate Transparency (crt.sh)

Extract subdomains from SSL certificates:

```
crt.sh → JSON parsing → cleanup → merge
```

---

## Output

```
passive_subdomains.txt
```

---

# 🧹 3. Data Normalization

## 🎯 Objective

Clean, unify, and structure all collected data.

---

## Process

```
merge sources
↓
lowercase normalization
↓
remove duplicates
↓
filter invalid domains
↓
final passive asset list
```

---

## Output

```
passive_all.txt
```

---

# 🌐 4. Service Discovery

## 🎯 Objective

Identify live web services from passive assets.

---

## Steps

### DNS Resolution

```
dnsx passive_all.txt
```

### HTTP Validation

```
httpx -status-code -title -tech-detect
```

---

## Output

```
live_services.txt
```

---

# 📊 5. Output Summary

At the end of Passive Recon we should have:

* Root domains validated
* Subdomains discovered
* Live services identified
* Attack surface expanded

---

## 🧠 Key Insight

> Passive Recon is not about finding everything — it is about finding the **right starting surface for deeper reconnaissance.**

---

# ➡ Next Phase

Once Passive Recon is complete:

→ Phase 2: Narrow Recon
→ Asset prioritization
→ URL discovery
→ parameter mining
