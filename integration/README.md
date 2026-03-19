# Trunk ↔ grounding integration

Trunk agents should send all turns through the grounding pipeline and verification layer so that outputs are auditable and non-hallucinatory.

## Usage from a Trunk agent

1. **System prompt**: Load the Trunk 2.0 (or other) system prompt when building the LLM context:

```js
import { getTrunkSystemPrompt, runTrunkWithGrounding } from "../integration/trunk-pipeline.js";

const systemPrompt = getTrunkSystemPrompt("2.0");  // reads trunk/prompts/trunk-2.0-system.md
// Use systemPrompt as the system message for your LLM; then run generation and pass output to runTrunkWithGrounding.
```

2. **After generating a reply** (or before submitting it), call:

```js
import { runTrunkWithGrounding } from "../integration/trunk-pipeline.js";

const { pass, report, verification } = runTrunkWithGrounding(
  "2.0",                    // trunk id
  "Patient reports lower back pain.",
  {
    candidateOutput: llmResponseText,
    sessionRef: encounterId,
    writeArtifacts: true,    // writes verification/report.json and evidence_tree.md
    useMcp: true,            // optional: use live docs + identity-au MCP retrieval (or set HEYDOC_USE_MCP=1)
  }
);

if (!pass) {
  // Block or flag: report.missing_receipts, verification.results
}
```

3. **Routing and retrieval** are still stubbed in `verification/pipeline.js`. To wire real MCP:
   - In your agent, call MCP tools (docs, identity-au, terminology, etc.) according to the plan from `runPipeline` (or a trunk-aware router).
   - Build the context packet with the returned receipts and citation IDs.
   - Pass that evidence into verification: use `verifyTrunkOutput(output, { citations, terminology_receipts, live_receipts })` or keep using `runTrunkWithGrounding` once the pipeline uses live MCP.

4. **Trunk-specific constraints** are in `integration/trunk-pipeline.js` (`TRUNK_CONSTRAINTS`). Add or change constraints there as trunks are implemented.

## First Trunk stub

A minimal stub that runs one turn through the pipeline lives in `trunk/stub-agent.js`. Run it with:

```bash
node trunk/stub-agent.js
```

It uses the integration layer and writes verification artifacts.
