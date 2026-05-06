# Three-Axis Vulnerability Diagnostic

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Governor — AI compliance oracle for Engineered Arts humanoid robots in public-facing roles. Three modes: (1) Live monitoring — streams real guest conversations and flags compliance risks in real time; (2) Simulate — operator types as a guest to QA, regression-test, or probe edge cases before going live; (3) Audit — upload a past transcript for batch compliance analysis and downloadable compliance records. Deployed in hotels, hospitals, museums, retail, and corporate front-of-house.
**Your Role:** Product Strategist

---

## Scores

### Contextual Moat — 4/5
*Workflow depth × switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:** Governor embeds into three distinct points in the deployment workflow — pre-launch QA (Simulate), live operations (Listen), and periodic compliance records (Audit). Each mode creates its own dependency: the Simulate mode becomes the regression test harness for every prompt change; the Listen mode becomes the operational dashboard the manager checks daily; the Audit mode produces the downloadable compliance records that go into the customer's legal files. Once those compliance records exist, switching tools means re-auditing everything from scratch. Switching cost is high and compounds over time. The moat is further reinforced by true behavioural parity with the physical robot and the plain-English scenario configurability — the robot's job description lives in Governor, not in a separate system.

**Named attacker:** A well-funded HRI (Human-Robot Interaction) startup building a generic robot testing layer across multiple hardware vendors — shallow parity but broad distribution.

---

### Data Advantage — 5/5
*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:** Every simulated and live conversation generates a proprietary dataset of how humans interact with embodied generative AI in real deployment contexts — receptions, classrooms, museums, hospitality. This includes edge cases, adversarial inputs, compliance-triggering topics, and out-of-role requests. No one else has this data. It compounds: the more deployments monitored, the better the compliance models become at flagging risk, which improves the product, which attracts more customers. OpenAI has no access to embodied, physical-world HRI data at this specificity.

**Named attacker:** OpenAI — if they partner with a hardware vendor and begin accumulating their own embodied conversation dataset at scale.

---

### Platform Exposure — 2/5
*Encroachment risk × pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:** The generic "AI conversation analytics" space is crowded and platform players could ship a surface-level version. However, the core defensive position is strong: true behavioural parity with the Engineered Arts robot is not replicable without owning the hardware and AI stack. The risk is not encroachment on the simulation layer but commoditisation of the compliance/monitoring layer if LLM providers build generic guardrail tooling (e.g. OpenAI's moderation API improving dramatically). Pivot speed is medium — the hardware integration is a meaningful barrier to fast imitation.

**Named attacker (compliance layer):** Anthropic or OpenAI shipping native compliance/guardrail tooling that enterprise customers layer on top of any AI system, reducing the perceived need for robot-specific monitoring.

**Named attacker (simulation layer):** ElevenLabs — their voice agent platform allows customers to rapidly prototype conversational AI personas with high-quality voice synthesis. For customers who undervalue the embodied layer and treat the robot as "just a voice agent with a face," a cheap ElevenLabs voice agent feels like a sufficient substitute for pre-deployment simulation testing. ElevenLabs cannot monitor live deployed robots or achieve embodied behavioural parity, but the perception risk is real. Defence: make the embodied difference empirically visible — surface cases where voice-only simulation gave a clean result but embodied simulation flagged a compliance risk.

---

## Top Vulnerability
Generic LLM guardrail tooling from foundation model providers commoditising the compliance monitoring layer — the defence must be that embodied, physical-world HRI creates qualitatively different risk that robot-specific data is uniquely positioned to catch.

## Confidence Level
H — genuine product gap confirmed within existing customer base, hardware moat is real, and compliance pain is urgent and recurring for both primary ICPs.
