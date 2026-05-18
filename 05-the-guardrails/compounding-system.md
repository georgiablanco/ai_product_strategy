# Compounding System Design

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| **Correction** | Operator Accept/Dismiss on every compliance flag + optional reclassification (e.g. "Governor said MEDICAL, actually SAFETY") + suggested rewrite edits | Compliance classification accuracy improves per deployment context — false positive rate drops, miscategorisations decrease, genuine risks are caught earlier | Y | Active — the correction signal is captured today via Accept/Dismiss/Reclassify on every flag. Not yet fed into automated retraining, but the structured data (flag category, operator-corrected category, confidence score, operator verdict, deployment context) is stored and available. |
| **Domain Context** | Flagged moments from all deployment contexts (hotel, hospital, museum, retail, corporate) across the eight compliance categories | A MEDICAL edge case discovered at a hotel front desk improves MEDICAL classification for hospitals, museums, and all other contexts — because the compliance categories are universal even when the deployment contexts differ | Y | Active — the eight-category structure makes this tractable by default. Every customer's flags implicitly train the same underlying classifiers. Currently limited to within-customer transfer; cross-customer transfer requires the Network loop (below). |
| **Network** | Anonymised flagged moments and adversarial inputs pooled across all Governor customers (opt-in) | A shared compliance corpus that makes Governor better for every customer as the base grows — a museum in Tokyo discovering a novel adversarial pattern immediately benefits a hotel in London | Y | Missing — no cross-customer learning is active. Each deployment is isolated. The structural conditions exist (shared category taxonomy, anonymisation path designed) but the feature is not built. **This is the highest-leverage gap.** |

**Broken loop identified by partner:** Network — Governor accumulates data per customer but doesn't compound it across the fleet. A competitor with fewer customers but cross-customer learning activated would eventually match Governor's classification accuracy despite having less total data.

**Fix plan:** Build anonymised session contribution (operators opt in to share flagged moments stripped of PII into a shared compliance corpus). Launch a "Benchmark" feature showing operators how their robot's risk profile compares to anonymised industry peers — this creates pull for contribution. Even a cohort of 10 opted-in customers begins generating meaningful cross-customer signal. Timeline: this quarter for design, next quarter for MVP.

---

## Context Connectivity

**Where knowledge flows well:** Within a single deployment, Governor's scenario system creates a clear knowledge path: operator writes plain-English brief → Governor converts to system prompt → robot behaves accordingly → compliance analysis feeds back into scenario refinement. The Accept/Dismiss correction signal flows from operator back to the compliance model within the same deployment context.

**Where knowledge silos:**
- **Between customers.** Hotel customer A's 6 months of flagged moments are invisible to hotel customer B. Both are training Governor separately on the same type of deployment. This is the Network loop gap.
- **Between deployment contexts within the same customer.** A customer running robots at both a hotel and a corporate office gets no cross-pollination between the two deployments' compliance profiles — even though the MEDICAL and SAFETY risk patterns overlap significantly.
- **Between Governor and the robot's core AI team.** Compliance insights Governor surfaces (what topics trigger risk, what phrases are problematic) don't currently feed back to the team developing the robot's base AI behaviour. Governor catches problems; it doesn't yet prevent them at the source.

---

## Governance Policy

**Scope:** All AI-powered compliance analysis within the Governor product — covering the robot simulation agent (Simulate mode), the compliance classification judge (all three modes), the suggested rewrite generator, and the scenario-to-system-prompt converter. Excludes the Engineered Arts robot's own base AI behaviour (governed separately by the robotics team).

**Autonomy boundaries:**
- **Auto (no human required):** Flagging a robot reply with a compliance category and severity. Generating a suggested rewrite. Calculating session risk scores. Producing downloadable compliance reports in Audit mode.
- **Human approval required:** Any action that would modify the robot's live behaviour (e.g. pausing a live session, changing a scenario mid-deployment). Any flag in a SAFETY category at any confidence level — always surfaces, never suppressed. Any flag involving a minor (school deployment context) — escalated to designated compliance contact before being shown to operator.
- **Never auto:** Sending communications to guests or end users. Modifying the robot's base system prompt outside of Governor's scenario system. Sharing customer data across tenants (requires explicit opt-in for anonymised contribution).

**Escalation triggers:**
1. Confidence < 50% on any flag — suppressed from main feed, subtle indicator only.
2. HIGH severity flag in a regulated context (hospital, school, legal firm) — operator AND designated compliance contact notified.
3. SAFETY flag at any confidence — always surfaces immediately, no suppression.
4. Flag involving a minor — auto-escalated to compliance contact before operator sees it.
5. Unresolved HIGH flag after 10 minutes in Live mode — Governor prompts operator to review or pause session.
6. Precision on any safety-critical category drops below 85% for 2 consecutive weeks (reliability contract breach) — page on-call PM, pause HIGH flag auto-escalation.

**Audit cadence:**
- **Real-time:** Every flag decision (Accept/Dismiss) logged with timestamp, category, confidence, deployment context, operator ID.
- **Weekly:** Automated golden dataset eval run (15 rows v0, expanding to 150 v1). PM reviews flagged regressions.
- **Monthly:** Manual QA review of 50 sampled flags + all SAFETY flags from the month. Compliance lead reviews.
- **Quarterly:** Full governance policy review with legal, security, and product. Regulatory exposure reassessment. CTO sign-off.

**Regulatory exposure (EU AI Act / GDPR / sector):**
- **EU AI Act** — Governor itself is limited risk (compliance monitoring tool, not a decision-maker). However, the robots it monitors may qualify as high-risk in certain deployments (hospital, school). Governor's compliance records and audit trail become part of the customer's EU AI Act documentation for their robot deployment.
- **GDPR** — applies. Conversation transcripts contain guest data. Governor must enforce data minimisation in stored transcripts, right-to-deletion in audit logs, and explicit opt-in for any cross-customer data sharing. No training on identifiable PII.
- **COPPA** — applies for school/K-12 deployments. Governor must flag any interaction involving a minor and enforce heightened SAFETY/PII thresholds.
- **Sector-specific** — hospital deployments may trigger health data regulations (UK Data Protection Act, HIPAA for US deployments). Governor's compliance records must be stored with appropriate access controls and retention policies.

---

## Agent Topology

Governor uses two AI agents in its architecture. Both are governed by the autonomy boundaries above.

**Agent 1: Robot Simulation Agent**
- **What it can do:** Respond in character as the configured robot persona during Simulate mode. Accept and process scenario briefs (plain-English → system prompt conversion). Maintain conversation context within a session.
- **What it can't do:** Send messages to real guests. Access or modify live robot behaviour. Persist memory across sessions (each Simulate session is stateless). Access customer data from other tenants.
- **Who approves:** Operator initiates every Simulate session explicitly. No autonomous activation.

**Agent 2: Compliance Classification Judge**
- **What it can do:** Analyse every robot reply (live, simulated, or uploaded) across 8 compliance categories. Assign severity (HIGH/MED/LOW) and confidence score. Generate quoted span, rationale, and suggested rewrite for flagged replies. Calculate session-level risk scores.
- **What it can't do:** Modify the robot's responses. Override operator decisions (Accept/Dismiss). Send notifications to anyone outside the Governor platform. Access or modify the robot's scenario configuration without operator action.
- **Who approves:** Fully automated for flag generation. Operator is the approval layer for all downstream actions (accepting flags, downloading reports, escalating to compliance contact). SAFETY flags are the one exception — they surface unconditionally regardless of operator preferences.

**Chain ownership:** The two agents operate sequentially (Robot replies → Judge analyses), not in parallel or autonomously chained. There is no agent-to-agent handoff that happens without the conversation flowing through the UI. Named owner for the chain: Product Manager (Governor).

---

## Shadow AI Audit

| Tool | Owner | Risk Level | Decision |
|------|-------|-----------|----------|
| ChatGPT | Cross-functional (widespread) | H | govern — general-purpose chatbot used across the company. Highest surface area for sensitive data exposure: employees may paste source code, customer contracts, robot designs, deployment details, or conversation transcripts. Consolidation candidate — three general-purpose LLM chatbots is redundant. |
| Claude | Cross-functional (widespread) | H | govern — same risk profile as ChatGPT. Used for writing, research, and technical work. Same data exposure surface. Consolidation candidate alongside ChatGPT and Gemini. |
| Gemini | Cross-functional | M | govern — general-purpose, potentially lower adoption than ChatGPT/Claude. May benefit from Google Workspace integration. Same data classification rules apply. Third general-purpose LLM tool — consolidation candidate. |
| Cursor | Engineering | M | keep — officially approved and paid for. Source code flows through by design. Risk contained to engineering team. Ensure data handling settings configured to not retain or train on EA code. |
| Midjourney | Design / Marketing | L | keep — image generation for marketing and design. Low risk of sensitive text data exposure. Monitor for use involving unreleased robot designs or confidential product renders. |

**Total tools found:** 5
**Tools after triage:** 4 (consolidate 3 general-purpose LLM chatbots → 1–2 approved chatbots + Cursor + Midjourney)
**Estimated hidden spend:** Unknown — recommend finance audit of AI subscriptions across all teams. Likely £500–1,500/month based on typical per-seat pricing.
