# 🧠 Recon Playbook

> A structured reconnaissance methodology for bug bounty hunting and offensive security research.

---

## 📌 Overview

Recon Playbook is a **real-world reconnaissance framework** built from practical bug bounty experience.
It focuses on transforming raw reconnaissance into a **systematic attack surface discovery process**.

Instead of just listing tools, this project defines a **complete workflow**:

* From passive intelligence gathering
* To asset discovery and validation
* To prioritization and deep attack surface mapping
* To threat modeling and manual testing preparation

---

## 🎯 Goals of this Project

* Build a **repeatable reconnaissance methodology**
* Reduce randomness in bug bounty hunting
* Turn recon into a **structured engineering process**
* Document real-world patterns and findings
* Provide reusable scripts and workflows

---

## 🧭 High-Level Workflow

```
Passive Recon
      ↓
Asset Discovery
      ↓
Validation & Enrichment
      ↓
Prioritization
      ↓
URL & Parameter Mining
      ↓
JavaScript Analysis
      ↓
Active Recon
      ↓
Threat Modeling
      ↓
Manual Testing
```

---

## 🧩 Core Philosophy

This project is built on a simple idea:

> Reconnaissance is not tool usage — it is decision-making.

Every step in this framework answers:

* What do we know?
* What can we infer?
* What should we investigate next?
* What is the highest-value target?

---

## 📂 Repository Structure

* `phases/` → Full recon methodology (step-by-step)
* `docs/` → Concepts and explanations
* `scripts/` → Automation utilities
* `field-notes/` → Real-world observations & case studies
* `diagrams/` → Workflow & architecture visuals
* `examples/` → Sample outputs from recon stages

---

## 🧪 What Makes This Different?

Most recon repositories look like this:

* Tools list
* One-liner commands
* Generic checklists

This project instead focuses on:

✔ Real-world workflows
✔ Attack surface thinking
✔ Prioritization strategies
✔ Pattern recognition
✔ Experience-based recon logic

---

## ⚙ Example Tools & Techniques (Non-exhaustive)

* Subdomain Enumeration (subfinder, amass, findomain)
* DNS & HTTP validation (dnsx, httpx)
* Web crawling (katana, hakrawler)
* Historical data mining (wayback, crt.sh)
* JS analysis & secret discovery
* Parameter extraction & fuzzing
* Scoring-based asset prioritization

---

## 📊 Field Notes

This repository includes real-world observations such as:

* Recon patterns discovered during bug bounty engagements
* Common misconfigurations in real systems
* Hidden asset discovery techniques
* JS-based endpoint extraction strategies
* API recon heuristics

---

## 🧠 Target Audience

* Bug bounty hunters
* Security researchers
* AppSec engineers
* Red team learners
* Offensive security enthusiasts

---

## 🚧 Status

This project is actively evolving as a **living reconnaissance framework**.

Future additions:

* Cloud recon module
* API recon expansion
* Automation pipeline improvements
* Advanced scoring models
* AI-assisted recon workflows

---

## 📜 License

MIT License (recommended for open research sharing)

---

## ⭐ Final Note

This is not a collection of tools.

It is a **way of thinking about reconnaissance**.
