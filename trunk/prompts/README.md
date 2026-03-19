# Trunk system prompts

System prompts for each Trunk agent. Load via `getTrunkSystemPrompt(trunkId)` from the integration layer when building the LLM context.

- **trunk-2.0-system.md** — Triage only; no diagnosis, no dosages; guideline claims must cite injected evidence.
- **trunk-3.0-system.md** — Structured history enrichment only; no diagnosis, no dosages; unknowns remain explicit.

- **trunk-4.0-system.md** — Problem representation and risk framing only; no diagnosis, no dosages; keep uncertainty explicit.
- **trunk-5.0-system.md** — Axis B deterministic rule-out framing; no diagnosis, no dosages; unknowns/missing proof explicit.
