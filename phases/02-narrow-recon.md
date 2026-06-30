# 🎯 Phase 2 — Narrow Recon (Target Refinement)

## 🎯 Goal

The goal of this phase is to **reduce noise and prioritize high-value attack surfaces** discovered in Passive Recon.

Instead of collecting more data, we now focus on:

* Filtering assets
* Prioritizing targets
* Identifying high-risk surfaces
* Building a focused attack plan

---

## 📥 Input

* `passive_all.txt`
* `live_services.txt`
* Subdomain enumeration results
* Historical URLs (Wayback / crt.sh)

---

## 📤 Output

* High-priority targets (Tier 1–3)
* Categorized assets (API / Admin / Legacy / Env)
* URL inventory for deep analysis
* Initial attack surface ranking

---

## 🧠 Core Idea

> Not all assets are equal. Recon value comes from prioritization, not volume.

---

# 🧭 1. Domain Prioritization (Scoring Model)

## 🎯 Objective

Assign value to discovered assets based on attack surface potential.

---

## 🏷 Asset Tagging

### Environment-based assets

```bash id="k1p9ab"
grep -Ei "(dev|test|stg|stage|uat|sandbox|preprod)" live_services.txt > env_targets.txt
```

---

### Admin / Internal surfaces

```bash id="p8xw2c"
grep -Ei "(admin|dashboard|console|internal|staff|root)" live_services.txt > admin_targets.txt
```

---

### API surfaces

```bash id="q7m1zt"
grep -Ei "(api|graphql|rest|v[0-9])" live_services.txt > api_targets.txt
```

---

CUSTOM FILTER LAYER (Yakan Methodology):
- apply noise reduction first
- apply wordlist-based enrichment
- apply anomaly detection before scoring

---

## 📊 Scoring Model

Each asset gets a score based on exposure type:

| Type                    | Score |
| ----------------------- | ----- |
| Admin Panel             | +4    |
| API Endpoint            | +3    |
| Environment (dev/stage) | +3    |
| Legacy tech             | +2    |
| Parameterized URL       | +2    |

---

## 🧮 Scoring Script

```bash id="s3xw9q"
awk '
FNR==1 {next}
{
  score=0
  if ($0 ~ /admin|dashboard|console/) score+=4
  if ($0 ~ /api|graphql|v[0-9]/) score+=3
  if ($0 ~ /dev|test|stg|stage|uat/) score+=3
  if ($0 ~ /php|asp|aspx|jsp/) score+=2
  if ($0 ~ /=/) score+=2

  print score "\t" $0
}' live_services.txt | sort -nr > scored_targets.txt
```

---

## 🏆 Tiering

### Tier 1 (Critical)

High-value targets for immediate testing:

```bash id="t9qk2a"
awk -F'\t' '$1>=9 {print $2}' scored_targets.txt > tier1.txt
```

---

### Tier 2 (Important)

```bash id="u1p7zd"
awk -F'\t' '$1>=6 && $1<9 {print $2}' scored_targets.txt > tier2.txt
```

---

### Tier 3 (Low priority)

```bash id="v4m8qz"
awk -F'\t' '$1<6 {print $2}' scored_targets.txt > tier3.txt
```

---

# 🌐 2. URL Discovery

## 🎯 Objective

Extract as many endpoints as possible from historical and live sources.

---

## 📜 Wayback URLs

```bash id="w2x9ka"
waybackurls example.com > wayback.txt
```

---

## 🧪 Crawling (Katana)

```bash id="x7m2qp"
katana -u example.com -d 5 -silent > katana.txt
```

---

## 🔗 Merge & Clean

```bash id="y3k8zn"
cat wayback.txt katana.txt | uro | sort -u > all_urls.txt
```

---

# 🌐 3. Advanced Dorking (Refinement Layer)

## 🎯 Objective

Find assets missed by automated enumeration.

---

```bash id="z1q6mv"
site:example.com -www
site:example.com inurl:admin
site:example.com ext:php
site:example.com ext:aspx
```

---

## 📦 Output

```text id="a9x2kc"
dork_urls.txt
```

---

# ⚙ 4. URL Scoring

Same idea as domains, but now for endpoints.

---

```bash id="b3m7qp"
grep -E "admin|api|internal|v[0-9]|debug|test" all_urls.txt > high_value_urls.txt
```

---

# 🧠 Key Insight

> Passive Recon finds assets.
> Narrow Recon decides where to attack first.

---

# 🚀 Transition to Next Phase

After this phase, we have:

* Prioritized attack surface
* High-value domains & endpoints
* Structured URL inventory

Next step:

👉 **Phase 3 — On Target Recon (Deep Investigation & Exploitation Surface Mapping)**
