# Governor — AI compliance oracle for Engineered Arts humanoid robots in public-facing roles. Three modes: (1) Live monitoring — streams real guest conversations and flags compliance risks in real time; (2) Simulate — operator types as a guest to QA, regression-test, or probe edge cases before going live; (3) Audit — upload a past transcript for batch compliance analysis and downloadable compliance records. Deployed in hotels, hospitals, museums, retail, and corporate front-of-house.

> A simulation and compliance monitoring platform for Engineered Arts robot deployments, giving enterprise operators a true-parity environment to red-team conversation behaviour and catch compliance violations before — and during — live deployment, generating proprietary HRI safety…

---

## Strategy at a Glance

| Component | Module | Status | Key Artifact |
|-----------|--------|--------|-------------|
| **The Bet** | M1 | [x] | `01-the-bet/` |
| **The Moat** | M2 | [x] | `02-the-moat/` |
| **The Margin** | M3 | [x] | `03-the-margin/` |
| **The Contract** | M4 | [x] | `04-the-contract/` |
| **The Guardrails** | M5 | [x] | `05-the-guardrails/` |
| **The Pitch** | M6 | [x] | `06-the-pitch/` |

---

## The Bet (M1)

**What we're building, for whom, why now.**

- **Product:** Governor — AI compliance oracle for Engineered Arts humanoid robots in public-facing roles. Three modes: (1) Live monitoring — streams real guest conversations and flags compliance risks in real time; (2) Simulate — operator types as a guest to QA, regression-test, or probe edge cases before going live; (3) Audit — upload a past transcript for batch compliance analysis and downloadable compliance records. Deployed in hotels, hospitals, museums, retail, and corporate front-of-house.
- **AI Value Archetype:** Oracle — the AI reveals compliance risk that is invisible to the deployment manager without systematic testing of the robot's generative responses.
- **Vulnerability Scores:** Moat 4/5 · Data 5/5 · Platform 2/5
- **Top Risk:** Generic LLM guardrail tooling from foundation model providers commoditising the compliance monitoring layer — the defence must be that embodied, physical-world HRI creates qualitatively different risk that robot-specific data is uniquely positioned to catch.
- **Confidence:** H
- **Prototype:**
- **Kill Criteria:** - Existing customers, when interviewed, don't describe compliance or unpredictable AI behaviour as a meaningful pain — they consider it already solved or not their problem - Customers are satisfied using a generic voice agent (e.g.…

→ Details: [`01-the-bet/`](01-the-bet/)

---

## The Moat (M2)

**Why this won't get copied in 6 months.**

- **Data Flywheel Score:**
- **Weakest Loop:** Network (2/5) — no cross-customer learning is currently active, meaning the data advantage is accumulating but not yet compounding across the customer base.
- **Top Encroachment Threat:** OpenAI / Anthropic
- **Encroachment Defense:** Build anonymised session contribution — operators opt in to share flagged moments (stripped of PII) into a shared compliance corpus.…
- **Vendor Portability:** Ready

→ Details: [`02-the-moat/`](02-the-moat/)

---

## The Margin (M3)

**Will this make money or bleed it?**

- **Gross Margin (current):**
- **Gross Margin (AI-adjusted):**
- **Pricing Model:** Service fee for feature access (Core bundled in contract; Live as add-on) + token consumption from the customer's existing allowance. Entirely consistent with EA's current commercial model — customers understand it, and the top-up bundle mechanism already handles heavy usage.
- **Pricing Today → Tomorrow:** Hardware sale + annual service/support contract + AI tokens per annum (usage-based) + top-up token bundles. Customers already have an established AI token relationship with Engineered Arts — Governor fits into this existing commercial structure rather than introducing a new billing model. → - **Governor Core** — bundled into the service contract as an uplift (e.g. +£300–500/robot/year). The robot simulation inference (the robot agent responding in Simulate and Audit modes) draws from the customer's existing token allocation — no new token cost to EA. EA's net new cost is the compliance classification layer only. Framed in renewal conversations as "AI compliance monitoring now included."
- **Total AI COGS / unit:**
- **Cascading Strategy:** Triage: Claude Haiku / GPT-4o-mini — fast, cheap, handles initial classification: "Is there a potential compliance issue in this robot reply?"; frontier: Claude Sonnet / GPT-4o — activated only when triage scores above risk threshold. Produces full flag output: quoted span, category, severity, rationale, and suggested rewrite.; ratio 75% triage-only / 25% frontier. Most robot replies in well-configured deployments are clean — the frontier model fires on edge cases and genuine risk. At scale, as the corpus of accepted/dismissed flags improves triage accuracy, this ratio should move toward 85/15.
- **Net Margin Shift:** Δ margin %: ~-5–10 points on the AI revenue layer vs. traditional hardware/service (65–70% vs. 70–75%) — but this understates the picture because EA's AI COGS is near-zero (infrastructure only, no inf…
- **Break-even at:**

→ Details: [`03-the-margin/`](03-the-margin/)

---

## The Contract (M4)

**Why users will trust a probabilistic system.**

- **Reliability Target:**
- **Golden Dataset:** 15 rows, 4 adversarial
- **Confidence UX:** Tiered confidence with explicit uncertainty labelling and operator override on every flag. Governor never fakes certainty — operators see the system's confidence level alongside every flag.…
- **HITL Architecture:** Governor is fully automated in v0 — every flag is surfaced directly to the deployment manager with no intermediate human review.…
- **Failure Mode Coverage:**

→ Details: [`04-the-contract/`](04-the-contract/)

---

## The Guardrails (M5)

**What breaks when this scales — and what compounds.**

- **Compounding System:** | Loop | Input | Output | Compounds? | Status | |------|-------|--------|-----------|--------| | **Correction** | Operator Accept/Dismiss on every compliance flag + optional reclassification (e.g.…
- **Governance Posture:** All AI-powered compliance analysis within the Governor product — covering the robot simulation agent (Simulate mode), the compliance classification judge (all three modes), the suggested rewrite generator, and the scenar…
- **Autonomy Boundaries:** - **Auto (no human required):** Flagging a robot reply with a compliance category and severity. Generating a suggested rewrite. Calculating session risk scores. Producing downloadable compliance reports in Audit mode.
- **Escalation Triggers:** 1. Confidence < 50% on any flag — suppressed from main feed, subtle indicator only.
- **Audit Cadence:** - **Real-time:** Every flag decision (Accept/Dismiss) logged with timestamp, category, confidence, deployment context, operator ID.
- **Shadow AI Audit (user-side):** 5 workarounds found · 4 (consolidate 3 general-purpose LLM chatbots → 1–2 approved chatbots + Cursor + Midjourney) build candidates · adjacent spend Unknown — recommend finance audit of AI subscriptions across all teams. Likely £500–1,500/month based on typical per-seat pricing.
- **Agent Boundaries:** Governor uses two AI agents in its architecture. Both are governed by the autonomy boundaries above.
- **Regulatory Exposure:** - **EU AI Act** — Governor itself is limited risk (compliance monitoring tool, not a decision-maker). However, the robots it monitors may qualify as high-risk in certain deployments (hospital, school).…

→ Details: [`05-the-guardrails/`](05-the-guardrails/)

---

## The Pitch (M6)

**How you get this funded, shipped, and adopted.**

- **Horizon 1 (Now):**
- **Horizon 2 (Next):**
- **Horizon 3 (Bet):**
- **Board Narrative:** **The case:**
- **Ask:** ## M1 Baseline vs. Now
- **Key Strategic Change:**

→ Details: [`06-the-pitch/`](06-the-pitch/)
