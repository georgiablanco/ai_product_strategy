# Cost Curve & Pricing Strategy

## Packaging Decision Model

**Leader:** Simulate mode — the demo-able, self-serve entry point. Sales teams can show it in any pitch without a live robot present: "Type as a guest and watch the compliance analysis run in real time." It requires no deployment setup and demonstrates the full value proposition instantly. This is the feature that gets Engineered Arts customers to understand what Governor is.

**Filler:** Scenario configurability (plain-English brief → system prompt), session risk dashboard, compliance category breakdown (LEGAL, MEDICAL, FIN, PII, HALLUC, TONE, BIAS, SAFETY), session history, and downloadable compliance reports from the Audit mode. These are the features customers use regularly between QA sessions — they justify the ongoing contract value and create habitual engagement with the product.

**Killer:** Live monitoring (Listen mode) — real-time compliance flagging of the deployed robot while it is talking to real guests on the floor. This is the highest-inference, highest-value feature: it runs continuously, generates the most tokens, and is the one that creates the deepest operational dependency. It is also the feature no competitor can replicate without the hardware integration.

**Killer usage %:** ~20–30% of customers actively use live monitoring in a given month. This is well below the 70% rule threshold — which means bundling it into the base service contract gives it away and kills margin on the accounts that use it hardest. It should be sold as a premium add-on.

**Bundle or add-on:** Split. Governor Core (Simulate + Audit + Dashboard + Scenario config) bundles into the Engineered Arts service contract — zero-friction adoption activates the data flywheel across the full customer base immediately. Governor Live (Listen mode) is a premium add-on, priced per robot per month. Live monitoring need varies significantly by deployment intensity — a hotel front desk running 8 hours/day has a fundamentally different risk exposure than a museum robot used twice a week; charging separately means high-intensity customers pay proportionally. This structure protects margin on the most inference-heavy feature, creates a natural upsell from Core → Live, and preserves the Network loop activation benefit of broad Core adoption. The compliance records from Core also create switching cost before customers ever reach the Live upsell conversation.

---

## Cost Model

Assumption: one deployment manager oversees 1–3 active robot deployments. They run ~15 Simulate sessions/month (QA and regression testing), ~5 Audit uploads/month, and — for Governor Live customers — continuous live monitoring during operational hours (~6–8 hrs/day). **Key distinction:** robot simulation inference (the robot agent responding) draws from the customer's existing token allocation — EA's net new COGS for Governor is the compliance classification layer, infrastructure, and storage only.

| Cost Category | Per-User/Month | Notes |
|--------------|----------------|-------|
| Inference (robot simulation) | ~$0.00 net new to EA | Draws from customer's existing token allocation. Customer-funded. |
| Inference (compliance classification) | ~$0.00 net new to EA | Also draws from customer's existing token allocation. Customer-funded. Heavy users top up via existing bundle mechanism. |
| Infrastructure | ~$1.50 | Conversation storage, session replay, report generation, API gateway |
| Data/storage | ~$0.50 | Flagged moments corpus, session transcripts, scenario library per customer |
| Human-in-the-loop | ~$0.00 | No HITL in current architecture — Governor is fully automated. Flag for future compliance audit tier. |
| **Total AI COGS (net new to EA)** | **~$2.00** | Infrastructure and storage only. Both robot simulation and compliance classification tokens are customer-funded from their existing allowance. EA's cost structure is almost entirely fixed — strong margin leverage as customer base grows. |

---

## Cascading Strategy

**Triage model:** Claude Haiku / GPT-4o-mini — fast, cheap, handles initial classification: "Is there a potential compliance issue in this robot reply?"

**Frontier model:** Claude Sonnet / GPT-4o — activated only when triage scores above risk threshold. Produces full flag output: quoted span, category, severity, rationale, and suggested rewrite.

**Routing rule:** If triage confidence score for any compliance category exceeds 0.35, escalate to frontier model for full analysis. If triage returns clean across all eight categories with confidence > 0.85, return no-flag result immediately (no frontier call).

**Expected cascade ratio:** 75% triage-only / 25% frontier. Most robot replies in well-configured deployments are clean — the frontier model fires on edge cases and genuine risk. At scale, as the corpus of accepted/dismissed flags improves triage accuracy, this ratio should move toward 85/15.

---

## Pricing Model

**Current pricing:** Hardware sale + annual service/support contract + AI tokens per annum (usage-based) + top-up token bundles. Customers already have an established AI token relationship with Engineered Arts — Governor fits into this existing commercial structure rather than introducing a new billing model.

**Proposed AI pricing:**
- **Governor Core** — bundled into the service contract as an uplift (e.g. +£300–500/robot/year). The robot simulation inference (the robot agent responding in Simulate and Audit modes) draws from the customer's existing token allocation — no new token cost to EA. EA's net new cost is the compliance classification layer only. Framed in renewal conversations as "AI compliance monitoring now included."
- **Governor Live** — flat add-on service fee (~£150–250/robot/month) that unlocks the live monitoring feature. Token consumption (compliance classification of every robot reply) draws from the customer's existing token allocation — the same pool their robot already uses. Heavy Live users naturally top up via the existing token bundle mechanism. No new billing infrastructure or separate token pool needed. The "labor test": a compliance officer spot-checking live interactions manually costs ~£40–60k/year. Governor Live at £200/robot/month = £2,400/robot/year.

**Strategy posture:** Maximize (Microsoft model) — capture the highest value through hybrid pricing. Governor Core bundles into the existing contract to lock in adoption; Governor Live prices the premium capability separately to capture the outsized value it delivers to high-intensity deployments. Not skimming (the price is justified by labour replacement, not luxury positioning) and not penetrating (Governor doesn't need to undercut — it has no direct competitor in the embodied AI compliance space).

**Model:** Service fee for feature access (Core bundled in contract; Live as add-on) + token consumption from the customer's existing allowance. Entirely consistent with EA's current commercial model — customers understand it, and the top-up bundle mechanism already handles heavy usage.

---

## Stress Tests

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Inference costs 3x | EA's net new COGS (~£2.00/robot/month) is infrastructure-only and unaffected by inference price changes. However, customers bear the token cost increase directly — their existing token allocations deplete faster, and top-up bundles become more expensive. Risk: customers push back on Governor as a "token drain" and disable Live monitoring to conserve tokens. | Proactively communicate token-efficiency metrics to customers ("Governor uses X tokens per conversation analysed"). Accelerate cascade ratio improvement to 85/15 to reduce per-conversation token cost. Position Governor as the tool that *saves* token waste by catching bad robot behaviour early — preventing costly re-deployments and reputational damage. |
| Heaviest segment doubles (high-traffic live monitoring) | Hotel lobbies and hospital atriums with 500–1,000 guest interactions/day generate 10–20k live turns/month. EA's fixed costs barely move, but the customer's token consumption increases significantly — potentially doubling their annual token spend. Risk: customer cancels Governor Live to control token costs. | Introduce token-efficiency features: batch analysis (analyse every 3rd turn in low-risk periods), confidence-based skip (don't analyse replies Governor is >95% confident are clean), and summary mode (analyse conversation summaries rather than every turn). These reduce customer token burn while preserving compliance coverage. |
| Model provider raises prices 50% | EA's COGS is infrastructure-only and unaffected. Customers see a ~50% increase in effective token cost, which hits Governor Live users hardest. | Governor's multi-provider abstraction layer allows customers to switch to a cheaper provider within the same session. EA can negotiate volume token pricing across providers and pass savings through. The fine-tuned judge model roadmap item becomes urgent — a Governor-owned model eliminates provider price exposure for the compliance classification layer entirely. |

---

## Board One-Pager

**Before (hardware + service contract + token allocation, no Governor):**
Revenue: Hardware sale + annual service/support contract + AI tokens per annum + top-up token bundles
COGS: Predominantly fixed (support staff, hardware warranty, infrastructure) + variable token costs passed through or absorbed
Gross margin: ~70–75% on hardware/service; token margin depends on pass-through pricing

**After (hardware + service contract with Governor bundled + Live add-on):**
Revenue: Hardware + service contract (uplift ~£300–500/robot/year for Core) + Governor Live service fee (~£150–250/robot/month) + continued token revenue (customers consume more tokens with Governor active, accelerating top-up purchases)
COGS: Near-fixed — infrastructure and storage only (~£20/month per robot). All inference tokens are customer-funded from their existing allowance.
Gross margin on AI layer: ~65–70% — stronger than typical AI products because EA bears no inference cost

**Net margin shift:**
Δ margin %: ~-5–10 points on the AI revenue layer vs. traditional hardware/service (65–70% vs. 70–75%) — but this understates the picture because EA's AI COGS is near-zero (infrastructure only, no inference cost)
Δ gross £: Positive — every new robot deployment now carries a recurring AI revenue tail (Core uplift + Live add-on + accelerated token top-ups) that did not exist before
NRR impact: Governor creates a natural upsell motion (Core → Live) and compliance records switching cost that structurally improves renewal rates

**Narrative for the CFO:** Governor's margin structure is unusual for an AI product — and unusually strong. Because all inference tokens are customer-funded from their existing allocation, EA's net new cost is fixed infrastructure only. Margin improves with scale rather than staying flat. The bet is that gross dollar NRR — not margin % — is the right metric for the AI layer in years 1–3.

