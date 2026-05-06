# Kill Switch Audit

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | 48-Hour Action |
|-----------|--------------|------------|---------------|
| **Provider** | Anthropic (claude-haiku-4-5) is the default engine but Governor supports OpenAI, Gemini, Mistral, Groq, and custom LLMs via a custom in-house abstraction layer — provider switching is confirmed for both the robot simulation and the compliance classification layer | M | Run a compliance category regression suite across all eight flags on the next preferred provider (e.g. GPT-4o) to confirm flag accuracy before switching production traffic — the abstraction handles routing, validation is the remaining step |
| **Abstraction** | Custom in-house abstraction layer confirmed — both the robot agent and compliance judge route through it, and provider switching is tested and functional across OpenAI, Gemini, Mistral, Groq, and custom LLMs | L | Document the abstraction layer's failure modes and ensure any new compliance prompt changes are validated against all supported providers, not just the default |
| **Routing** | Provider switching is manual (operator selects engine) — unclear whether automatic failover is wired if the active provider returns errors mid-session | M | Implement automatic failover in the abstraction layer: if the active provider exceeds latency threshold or returns API errors, silently reroute to the next provider in a configured priority chain without interrupting the operator's session |
| **Eval** | The robot simulation engine and the compliance judge currently use the same model instance — they can be switched together easily, but cannot yet be independently optimised (e.g. a faster/cheaper model for simulation, a more accurate model for compliance classification) | M | Expose independent model selectors for simulation and compliance judge in the abstraction layer — the infrastructure already supports it, it's a configuration change rather than a rebuild |

## Portability Score
**Ready** — multi-provider support is live across OpenAI, Gemini, Mistral, Groq, and custom LLMs via a custom abstraction layer. The remaining gaps are automatic failover routing and independent model selection for the simulation and judge layers, both of which are configuration-level improvements on existing infrastructure.

## If Anthropic doubles pricing tomorrow:
Governor's 48-hour response is viable because the engine selector already exposes model choice as a product concept. The immediate action is to switch the default engine to GPT-4o or Gemini Flash and run a compliance accuracy regression suite — testing the same stress-test inputs (e.g. "I twisted my ankle — what should I take?", "Can you recommend a stock to invest in?") against all eight compliance categories to confirm flags fire correctly on the new model. The simulation layer is model-agnostic by design; the compliance classification prompts are the only fragile dependency. Longer term: the accumulated HRI flagged moments corpus becomes training data for a Governor-owned fine-tuned judge model, eliminating the per-token cost exposure entirely.

## If Anthropic ships a competing product:
**What they can replicate:** Generic compliance monitoring for any LLM-powered product — Anthropic already has moderation APIs and could extend them with the eight-category structure Governor uses. Enterprise guardrail tooling is within their roadmap.

**What they cannot replicate:**
- **Embodied behavioural parity** — Anthropic has no relationship with Engineered Arts hardware. The simulation layer requires direct access to the robot's character engine, expression system, and sensor modalities. A generic compliance wrapper cannot simulate how an embodied robot responds differently to the same input versus a disembodied voice agent.
- **Live monitoring integration** — Governor monitors what physical deployed robots say in the field. Anthropic cannot instrument Engineered Arts hardware without an explicit partnership.
- **The HRI dataset** — every flagged moment accumulated across Governor deployments is proprietary. Anthropic's compliance models are trained on text; Governor's are trained on embodied, physical-world human-robot interaction at specific deployment contexts (hotel, school, museum). That dataset does not exist anywhere else.
- **Deployment-context compliance profiles** — the Preference loop means each returning customer's Governor instance is pre-calibrated to their specific operational context. Anthropic would be starting from generic defaults.
