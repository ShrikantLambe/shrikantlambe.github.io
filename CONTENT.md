# CONTENT.md — new copy for the Governed-by-design redesign

Written without a formal AUDIT.md (P1 needs Playwright, not available this session) — drawn instead from direct, verified knowledge of the current site, resume, and project repos built up over many prior sessions on this project. Treat anything not already published on the live site or in the resume as needing your confirmation, not as settled fact. No numbers in this document are invented — every figure here already appears on shrikantlambe.vercel.app or in the resume; flagged below wherever I'm not certain it's safe to repeat.

## Hero

Three one-line identity options, built on "semantic layers + agentic AI," max 14 words, no buzzwords, no "passionate":

1. **"Semantic layers and agentic AI — governed data that dashboards and agents both trust."** (13 words)
2. **"I architect the semantic layer that Cortex agents and BI dashboards both run on."** (14 words)
3. **"Fifteen years turning enterprise data into one governed layer AI agents can trust."** (13 words)

### Hero rail
- **Role:** Solutions Architect, Data & AI (Contract) — Intuitive Surgical
- **Location:** Milpitas, CA · Remote-friendly
- **Availability:** [ASK — current site says "open to senior & staff roles"; this pack's own review-persona targets "Principal Data & AI Engineer." Confirm the target level before I lock this line.]

---

## Case studies (flagship)

### 1. Semantic layer for manufacturing AI agents
**Provenance: Production, Intuitive Surgical, 2026**

- **Problem:** Operations leaders needed natural-language access to manufacturing metrics that previously required an analyst and a hand-written SQL query. Metric definitions — e.g. On-Time Delivery — were duplicated across stored procedures, database views, and Tableau extracts, with no single source of truth.
- **My role:** Solutions Architect, Data & AI (contract) — architected and shipped the semantic layer and Cortex agent platform.
- **Architecture:**
  - SAP ECC modeled into Snowflake semantic views across 5 operational domains (Inventory, Manufacturing, Procurement, Order Fulfillment, Quality)
  - 8 semantic views consolidate metric definitions previously duplicated across stored procedures, database views, and Tableau extracts
  - One Cortex Analyst contract serves Tableau, 5 production Cortex agents, and power users from a single governed definition
  - Snowflake query history and Tableau usage analysis prioritized which metrics and join paths to model first
- **Decisions and trade-offs:**
  1. One governed Cortex Analyst contract serving all three consumer types, instead of a separate semantic layer per tool. Trade-off: more upfront modeling discipline, in exchange for eliminating the metric-definition drift that caused the original problem.
  2. Prioritized modeling using actual query/usage history instead of a full upfront domain model. Trade-off: narrower initial scope (5 domains, not all of SAP ECC), but every shipped view maps to verified real consumption.
- **Outcome (system, scale, before → after):** Semantic layer over SAP ECC, 5 operational domains. Before: metric definitions duplicated across stored procedures, views, and Tableau extracts, no single source of truth. After: 8 governed semantic views serving Tableau, 5 production Cortex agents, and power users from one definition.

**[ASK — confidentiality sign-off required before this goes live, per your own instruction in CLAUDE.md.]** Everything above is already on the live site's About/Experience copy or the resume; nothing new is disclosed. Flag anything you want generalized further or removed outright.

### 2. Pipeline Sentinel
**Provenance: Personal build, synthetic data**

- **Problem:** Pipeline failures in Airflow require a human to get paged, diagnose, and manually remediate — repeat incidents redo the same diagnostic work every time.
- **My role:** Independent build, designed and built solo.
- **Architecture:**
  - 5 specialized agents (Monitor, Diagnosis, Blast Radius, Remediation, Reflection) coordinate through shared state
  - Blast-radius agent scores downstream impact via BFS over the dependency graph before any remediation runs
  - A confidence gate auto-remediates only low-risk, high-signal cases; everything else escalates with a full audit trail
  - Pattern memory fast-paths known failures after repeat successful fixes; a reflection loop retries with a different strategy on failure
- **Decisions and trade-offs:**
  1. Auto-remediate only above a confidence threshold, rather than attempt full autonomy. Trade-off: some fixable incidents still escalate to a human, in exchange for keeping false-autofix risk near zero.
  2. Score blast radius via graph traversal before remediating. Trade-off: added per-incident latency, in exchange for not fixing a symptom while a wider-impact root cause goes unaddressed.
- **Outcome:** Demo-scenario resolution in ~11s at 94% confidence. This is the project's own test/demo scenarios, not measured production incident volume — stated here instead of as an inline caveat, per the provenance note above.

### 3. LoanLens
**Provenance: Personal build, synthetic data**

- **Problem:** Lending teams need portfolio intelligence — reconciliation, covenant monitoring, investor reporting — that's auditable and fast, not spreadsheet-reconciled after the fact.
- **My role:** Independent build, shipped as a focused sprint.
- **Architecture:**
  - 10,000 simulated loans and 1.35M payment events modeled in dbt across staging, intermediate, and mart layers
  - Warehouse-portable design swaps DuckDB for Snowflake by config, same dbt models either way
  - End-to-end reconciliation pipeline checks payment events against loan terms to <0.1% tolerance
  - Claude layer emits structured JSON for anomaly detection and generates investor-memo narratives from the reconciled data
- **Decisions and trade-offs:**
  1. Reconciliation built as a dbt test/model layer, not a one-off script. Trade-off: more setup work, in exchange for reconciliation rerunning automatically with every model build.
  2. Synthetic loan/payment data instead of a real anonymized portfolio. Trade-off: not real-world-noisy data, in exchange for zero confidentiality exposure while still exercising the full reconciliation and covenant logic.
- **Outcome (3 numbers max):** 10,000 simulated loans, 1.35M payment events, reconciled to <0.1% tolerance. Before: manual spreadsheet reconciliation and ad hoc investor memos. After: automated reconciliation with Claude-generated investor memos on demand.

---

## Other builds (one line each, max 18 words)

- **HR Analytics Copilot** — Recruiting dashboard where an LLM writes and runs its own SQL in-browser, with a citation-grounded fallback.
- **AI-Native Data Platform on Snowflake Cortex** — Reference architecture for governed AI inside Snowflake Cortex, with a live interactive demo.
- **Coffee Barista Agent** — Google ADK ordering agent grounded in an actual menu; refuses off-menu recommendations, respects allergen tags.
- **Growth Intelligence Agent** — RAG-grounded revenue-analyst agent that detects anomalies and cites playbooks across 6 SaaS growth metric categories.
- **Real-Time Retail Sales Pipeline** — Kafka-to-Snowflake streaming pipeline with dbt incremental models, load-tested at 25 events/sec in Docker Compose.
- **Marketing AI Intelligence Engine** — 4-layer GenAI pipeline turning campaign data into executive summaries, inspired by production AI work at Workday.
- **Customer Churn Analysis & Prediction** — SHAP-explainable churn model with LLM-generated, per-customer retention strategy served via FastAPI.
- **GPT-4o AI Chatbot** — Multi-turn chatbot baseline deployed on Render. **Suggest dropping** — least differentiated build; doesn't show anything the agentic projects don't show better.
- **Kids AI Explorer** — 4-week AI-literacy curriculum for kids 6–12, live on Vercel with Gemini/Claude-powered chat. **Suggest dropping from this site** (strong work, wrong audience) — consumer/EdTech product reads as scope-dilution against a Staff/Principal Data & AI pitch.
- **Marginalia** — Paste a URL, get a TL;DR and a saved reading list — zero-cost, self-hostable stack.
- **KinSync** — Family scheduling hub with a Vertex AI Reasoning Engine assistant and live Maps route cards via A2UI. **Suggest dropping from this site** for the same reason as Kids AI Explorer.

**[ASK]** — "drop" above means cut from this redesign entirely, not just demote further. Confirm, since it's a real deletion of content people may have linked to.

---

## Writing (3 essays + series link)

Picked for closest fit to the semantic-layer/agentic-AI identity:

1. **"What It Actually Takes to Run AI Natively Inside a Data Warehouse"** — most direct match: governed AI inside the warehouse, same territory as the Intuitive case study.
2. **"Agentic Data Infrastructure: I Built a Self-Healing Data Pipeline System"** — direct companion piece to the Pipeline Sentinel case study.
3. **"I Built a Growth Intelligence Agent That Acts as a Virtual Analyst"** — reinforces the agentic-AI half of the identity.

Plus: link to the Semantic Layer series (`tdb_semantic_layer_series/`).

---

## Experience (one outcome line per role, current role first; 6 rows)

1. **Intuitive Surgical** — Solutions Architect, Data & AI (Contract), May 2026–present. *Semantic layer over SAP ECC — before: metric definitions duplicated across systems; after: 8 governed views serving Tableau and 5 production Cortex agents.* **[Production, Intuitive Surgical, 2026]**
2. **Workday** — Manager, BI Analytics, Apr 2020–Apr 2026. *Sales & Marketing analytics platform — before: fragmented Tableau reporting, inconsistent numbers across teams; after: governed Sigma/Snowflake self-service platform, ~40% less manual reporting.* **[Production, Workday, 2020–2026]** — [ASK: confirm the 40% figure is still OK to carry forward; it's already on the live site.]
3. **Lyft** — Senior BI/Data Engineer, Growth Analytics, 2018–2020. *Growth analytics infrastructure — before: ad hoc acquisition-spend tracking; after: pipelines powering CAC/ROI/LTV metrics behind multi-million-dollar spend decisions.* **[Production, Lyft, 2018–2020]**
4. **Intuitive Surgical** — Senior BI/Data Engineer, Supply Chain, 2016–2018. *Supply chain & manufacturing analytics — before: manual Excel reporting; after: production pipelines and governed dashboards for lead times, defect rates, capacity.* **[Production, Intuitive Surgical, 2016–2018]**
5. **ServiceNow** — Lead BI/Tableau Analyst, 2015–2016. *Enterprise dashboards across Finance, Sales, HR, Product during rapid growth — standardized metrics, eliminated recurring manual reports.* **[Production, ServiceNow, 2015–2016]**
6. **Juniper Networks** — Lead BI Developer, 2014–2015. *Reporting strategy through an SAP CRM migration — owned SIT/UAT coordination and user enablement.* **[Production, Juniper Networks, 2014–2015]**

---

## [ASK] — questions before this content is final

1. **Confidentiality sign-off** on the Intuitive Surgical case study as scoped above — everything in it is already public on the live site/resume, but you asked to review before anything about a current client goes live. Anything to generalize further or cut?
2. **Which hero identity line** (1, 2, or 3 above), or none of them — want a different direction?
3. **Target seniority for the availability line** — "staff," "staff & principal," or something else?
4. **Confirm "drop" means delete**, not demote, for GPT-4o Chatbot, Kids AI Explorer, and KinSync.
5. **Workday's 40% figure** — OK to reuse in the new Experience line? (Already live on the current site; flagging since this content pack treats re-use of any number as something to confirm, not assume.)
6. Anything in Pipeline Sentinel's or LoanLens's outcome lines you want added or cut, given the "max 3 numbers" rule?
