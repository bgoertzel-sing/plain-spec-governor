# Plain Spec Governor Extension Proposal: Philosophical Commitments

## Purpose

Some software specifications encode more than local behavior. They can quietly commit the project to an ontology, epistemic stance, governance model, memory theory, permission model, or scientific norm. If that commitment remains implicit, a later coding agent can make a locally reasonable simplification that preserves the surface feature while reversing the deeper point.

This proposal adds a light-touch way for Plain specs and the `PlainSpecReviewer` to expose those hidden commitments without turning ordinary engineering into philosophy seminar bureaucracy.

The motivating pattern is:

> "Claude Tag is not mainly about Slack; it exposes questions of identity, memory, attribution, permissions, and live oversight."

For OpenClaw/OmegaClAW work, similar hidden commitments appear in designs such as inspectable memory, cost-aware routing, PeTTa experiment records, ThreadKeeper delegation, and GGB capacity gates.

## Summary of Proposed Changes

1. Add an optional Plain section named either `***design rationale***` or `***philosophical commitments***`.
2. Add a reusable `:PhilosophicalCommitment:` concept.
3. Extend intensive review with a concealed-philosophy pass, normally for Tier 2 and Tier 3 work.
4. Add small lint checks for high-risk specs: if the design affects identity, memory, attribution, permissions, oversight, epistemic trust, scientific evidence, or agency boundaries, the spec should either contain commitments or explicitly say none were found.
5. Add acceptance test patterns that protect commitments from regression, without requiring tests to prove philosophical claims.
6. Keep the feature optional for Tier 0-1 and non-blocking unless an implicit commitment directly affects safety, research validity, governance, or persistent architecture.

## New Plain Section

### Preferred Section Names

Plain specs may include one of these equivalent sections:

```plain
***design rationale***
```

or:

```plain
***philosophical commitments***
```

Use `***design rationale***` when the commitments are mostly practical architectural motivations.
Use `***philosophical commitments***` when the commitments are explicitly about identity, memory, knowledge, trust, agency, permissions, evidence, interpretability, or governance.

For public or collaborator-facing specs, `***design rationale***` may be less intimidating while still carrying the same semantics.

### Syntax

Each commitment should be a concise bullet. Recommended shape:

```plain
***philosophical commitments***

- :PhilosophicalCommitment: PC-001 — Inspectable memory is preferred over opaque memory for durable project knowledge.
  Rationale: Future agents and humans must be able to audit what is remembered, why it is trusted, and where it came from.
  Protects: :AgentState:, :Artifact:, :SafetyBoundary:
  Design implications:
  - Persistent knowledge should be represented in files, records, ledgers, or queryable atoms rather than only in hidden model context.
  - Summaries must preserve provenance pointers.
  Non-goal: This does not require every transient conversational detail to be stored.
  Regression risk: Replacing durable records with uninspectable context would satisfy short-term UX but violate this commitment.
```

Short form is allowed:

```plain
***design rationale***

- PC-001: Prefer explicit run-contract records over unstructured logs because scientific claims must be replayable and inspectable.
```

### Semantics

A `:PhilosophicalCommitment:` is not a poetic note. It is a durable design constraint explaining why some behavior, boundary, or representation matters beyond local convenience.

It should answer at least one of:

- What deeper stance does this design encode?
- What future simplification would accidentally betray the point?
- Which concepts or requirements exist mainly to preserve this stance?
- What evidence, test, invariant, or review gate can detect regression?

A commitment may influence implementation, tests, modularity, and review severity, but it should not create vague untestable obligations. If it has no practical implication, it belongs in project notes, not the Plain spec.

### Recommended Fields

For Tier 2-3 specs, each important commitment should include some subset of:

- `Rationale:` why this matters.
- `Protects:` Plain concepts or requirements tied to the commitment.
- `Design implications:` concrete constraints on implementation choices.
- `Non-goal:` what the commitment does not require.
- `Regression risk:` how a future change could appear reasonable while violating the commitment.
- `Acceptance hook:` a test, review check, invariant, or artifact that guards it.

Do not require every field mechanically. The reviewer should prefer one useful paragraph over a rigid template.

## Reusable Plain Concept

Add to the reusable concepts list, probably under a new heading "Rationale/governance concepts":

```plain
- :PhilosophicalCommitment: is an explicit design commitment about ontology, epistemic trust, agency, identity, memory, attribution, permissions, oversight, evidence, interpretability, or governance that materially constrains implementation or future revisions.
```

Optional companion concepts for later, not required now:

```plain
- :DesignRationale: is the reason a requirement or architecture choice exists, especially when the reason is not obvious from behavior alone.
- :RegressionRisk: is a plausible future change that would satisfy surface requirements while violating an intended commitment.
```

Recommendation: add only `:PhilosophicalCommitment:` initially. `:DesignRationale:` and `:RegressionRisk:` are useful words but may add concept clutter.

## Reviewer Changes

### Persistent Reviewer Mission

Update `PlainSpecReviewer` memory responsibilities with:

- Remember recurring philosophical commitments behind OpenClaw/OmegaClAW designs, such as inspectability, provenance, attribution, permission boundaries, live oversight, bounded delegation, and evidence-first research claims.
- Notice when a local implementation choice could reverse a project-level commitment.

Update review stance with:

- When a spec concerns Tier 2 or Tier 3 work, identify concealed philosophical commitments that may affect architecture, tests, acceptance gates, or future compatibility.
- Do not over-philosophize trivial utilities. Propose commitments only when they change what should be built or preserved.

### New Intensive Review Pass

Insert a new pass after "PASS 3 — Ambiguity and Misimplementation Audit" or after "PASS 6 — Framework and Modularity Audit":

```text
PASS X — Concealed Philosophy / Design Rationale Audit
Check whether the spec encodes implicit commitments about identity, memory, attribution, permission, oversight, agency boundaries, epistemic trust, scientific evidence, interpretability, governance, or ontology.

For each plausible hidden commitment:
- name the commitment in plain language;
- identify which requirements or concepts imply it;
- explain how a coding agent or future maintainer could satisfy surface requirements while violating it;
- decide whether it should be added to `***design rationale***` or `***philosophical commitments***`;
- propose the smallest acceptance hook, invariant, or review note that protects it.

Avoid abstract commentary unless it changes the spec, tests, architecture, or implementation gate.
```

Add `Philosophical-commitment` to issue table categories.

Add a new output section for intensive reviews:

```markdown
# Concealed Commitments and Rationale
- Commitment:
- Implied by:
- Why it matters:
- Regression risk:
- Recommended Plain addition:
```

If changing the output format is too disruptive, fold this into `# Issue Table` and `# Proposed Plain Revision`. However, an explicit section is useful because otherwise these points get buried as ordinary ambiguity issues.

### Light Review Prompt

For Tier 0-1 light review, add only a single optional sentence:

```text
If the task unexpectedly affects persistent memory, permissions, attribution, user oversight, or scientific evidence, note any hidden design rationale that should be made explicit; otherwise do not add philosophical overhead.
```

## Risk Tier Interaction

### Tier 0 — Tiny Local Helper

Normally no `***design rationale***` or `***philosophical commitments***` section.

Exception: if the helper transforms evidence, deletes provenance, rewrites memory, or feeds later research claims, add one short design-rationale bullet or bump to Tier 2/3.

### Tier 1 — Setup or Command-Line Script

Usually optional. Consider a short rationale when the script affects:

- user/system permission boundaries;
- persistent configuration;
- credential hygiene;
- auditability or rollback;
- operator oversight.

Example: a setup script that refuses `curl | sh` is not merely conservative shell style; it encodes a trust/provenance commitment.

### Tier 2 — Codebase Revision or Agent Behavior Change

Recommended by default when the change touches:

- agent memory or persistence;
- identity/session continuity;
- inter-agent delegation;
- user permissions or tool boundaries;
- model routing or budget governance;
- attribution and audit records;
- live oversight / human-in-the-loop behavior;
- prompt/context boundaries.

A missing commitment should usually be `Major` if it risks future architecture drift, and `Critical` only if implementation could immediately corrupt state, bypass permissions, or undermine oversight.

### Tier 3 — Long-Running Research Code

Recommended by default when the code supports scientific claims or research interpretation. Typical commitments:

- evidence should be inspectable and replayable;
- known-answer tests precede claims;
- negative/control cases matter;
- generated artifacts are not just logs but epistemic support;
- abstractions should preserve future MeTTa/MM2/Hyperon/Atomspace portability.

A missing commitment should be `Research-risk` when it mainly affects interpretation/publication, `Major` when it affects architecture, and `Critical` only if it invalidates required acceptance tests or safety gates.

## New Lint Checks

Add to local Plain lint for Tier 2-3:

1. **Commitment-section trigger check**
   - If the spec uses concepts such as `:AgentState:`, `:SafetyBoundary:`, `:ExternalAction:`, `:Artifact:`, `:Metric:`, `:ExperimentRun:`, memory, provenance, attribution, permission, oversight, identity, delegation, cost, budget, or audit, then either:
     - include `***design rationale***` / `***philosophical commitments***`, or
     - include a reviewer note that no material hidden commitment was found.

2. **Commitment practicality check**
   - Each `:PhilosophicalCommitment:` should connect to at least one functional spec, implementation requirement, test requirement, acceptance test, invariant, or explicit non-goal.
   - Flag purely decorative commitments as Minor.

3. **Regression-risk check**
   - For Tier 2-3 commitments, at least one bullet should identify a plausible future regression or simplification that would violate the commitment.
   - This can be inline; no rigid field required.

4. **Contradiction check**
   - Flag a contradiction when a functional spec implies one philosophy but implementation requirements imply another.
   - Example: "all memory must be auditable" plus "store durable state only in provider conversation memory".

5. **Overreach check**
   - Flag commitments that assert broad metaphysical claims not needed for the implementation.
   - The section should protect design intent, not settle philosophy.

6. **Naming check**
   - If `:PhilosophicalCommitment:` is used as a concept, it must be defined.
   - Commitment IDs like `PC-001` should be unique within the spec.

These can be implemented first as reviewer checklist items rather than a parser.

## Acceptance Test Patterns

The goal is not to test philosophy directly. The goal is to test the operational consequences of a commitment.

### Pattern 1 — Commitment-to-Requirement Trace

For each important `:PhilosophicalCommitment:`, at least one acceptance test or review note should name the requirement that protects it.

Example:

```plain
- PC-001: Durable memory should be inspectable and provenance-bearing.

- The :AgentModule: shall write promoted memory to a human-readable record with source pointers.
  ***acceptance tests***
  - Given a promoted memory item, the record includes source path/date and can be read without invoking an LLM provider.
```

### Pattern 2 — Surface-Compliant Regression Test

Create a test where a naive implementation satisfies the obvious feature but violates the commitment.

Example:

```plain
- Given an agent delegation completes successfully, the parent receives a summary.
- Regression acceptance hook: the run also persists a transcript pointer and checksum; summary-only delegation is nonconformant.
```

### Pattern 3 — Provenance / Audit Hook

For commitments about epistemic trust, require artifact checks:

```plain
- After an :ExperimentRun:, the artifact directory contains configuration, seed, source commit, metrics, and control-case status.
- A result summary without these artifacts cannot support the research claim.
```

### Pattern 4 — Permission / Oversight Hook

For commitments about agency boundaries:

```plain
- Given a child/subagent proposes a patch under `patch_proposal_only`, workspace files are not modified until a parent/operator applies the patch.
```

### Pattern 5 — Portability / Ontology Hook

For commitments about representation:

```plain
- Given a run record, it can be projected into both a Markdown summary and a structured atom/JSON fixture without losing required fields.
```

## Concrete Examples

### 1. Intent Model Router / Cost-Aware Fable Routing

Surface spec: route prompts among routine, deep, and Fable models; track cost; enforce a daily budget.

Hidden commitments:

- Strong model use is a governed scarce resource, not just a quality knob.
- Human approval is required for same-day budget override.
- Cost tracking must be inspectable without storing private prompt/reply content.
- Default behavior should avoid expensive/opaque escalation unless justified.

Possible Plain addition:

```plain
***philosophical commitments***

- :PhilosophicalCommitment: PC-001 — Model intelligence is a governed resource, not an automatic entitlement.
  Rationale: The router should preserve user/operator oversight over expensive or strategically important model calls.
  Protects: :FableMode:, :DailyBudget:, :HumanOverride:, :RoutingDecision:
  Design implications:
  - Automatic Fable routing must be disabled by default.
  - Over-budget Fable routing requires a same-day human-approved override.
  - Deprecated config that bypasses this boundary must be ignored or treated conservatively.
  Regression risk: A future change that routes all hard prompts to Fable by default may improve local answer quality but violate cost/oversight governance.

- :PhilosophicalCommitment: PC-002 — Cost/accounting records should be auditable without becoming surveillance records.
  Rationale: The system needs spend visibility while preserving privacy and credential hygiene.
  Protects: :CostLedger:, :UsageEvent:, :BudgetSummary:
  Design implications:
  - Ledger entries must not include raw prompts, replies, API keys, headers, environment values, or file contents.
  - Malformed ledger lines should not erase or rewrite audit history.
  Regression risk: Adding prompt text to simplify debugging would violate the privacy/accounting boundary.
```

Acceptance hooks that would follow:

- Given `fableMode: off`, explicit Fable requests do not route to Fable.
- Given over-budget status without same-day `:HumanOverride:`, Fable routing falls back to deep model.
- Ledger entries do not contain prompt or assistant text.
- Malformed ledger lines are skipped, not rewritten or used to crash routing.

How this changes the spec: the existing functional behavior already mostly protects these commitments, but the rationale would make future changes less likely to "optimize" away human override or privacy-preserving ledger constraints.

### 2. petta-chem / PeTTa Algorithmic Chemistry Experiments

Surface spec: build deterministic PeTTa-native abstract chemistry experiments with planted ACS recovery, controls, ablation, run-contract records, and dynamic random/control families.

Hidden commitments:

- Scientific claims should rest on replayable evidence, not only persuasive summaries.
- PeTTa/MeTTa-native representation matters because the research is about symbolic/cognitive substrate, not just convenient Python simulation.
- Controls and ablations are part of the ontology of the result, not afterthought plots.
- Run records are epistemic objects: config, manifest, events, metrics, candidates, controls, and caveats must travel together.

Possible Plain addition:

```plain
***philosophical commitments***

- :PhilosophicalCommitment: PC-001 — Experiment results are evidence records, not just outputs.
  Rationale: Claims about autocatalytic structure or emergent chemistry must be traceable to reproducible configs, events, metrics, controls, and ablations.
  Protects: :ExperimentRun:, :Artifact:, :Metric:, :SyntheticCase:, :Baseline:
  Design implications:
  - Each run family should produce structured run-contract atoms or equivalent inspectable records.
  - Summaries must preserve links to config, manifest, events, metrics, ACS candidates, controls, and caveats.
  - A result without controls/ablations cannot support a strong emergence claim.
  Regression risk: Replacing run contracts with a single aggregate CSV or narrative summary would speed reporting but weaken the epistemic status of the result.

- :PhilosophicalCommitment: PC-002 — Native symbolic representation is part of the research claim.
  Rationale: The project is not merely simulating chemistry; it is testing whether PeTTa/MeTTa-shaped structures can host inspectable algorithmic chemistry patterns.
  Design implications:
  - Python helpers may serialize or check artifacts, but core experiment semantics should remain representable in PeTTa/MeTTa atoms where practical.
  - Future MORK/Hyperon/Atomspace migration seams should not be blocked by host-language shortcuts.
  Regression risk: Moving all semantics into opaque Python data structures could preserve numeric outputs while abandoning the symbolic-substrate question.
```

Acceptance hooks:

- Given a known planted ACS fixture, scanner recovery and ablation productivity-drop checks pass.
- Given shuffled/no-catalysis controls, active ACS candidates remain zero or match expected negative behavior.
- Given an experiment run, required run-contract files/atoms exist and include config, manifest, events, metrics, candidates, controls, summary, and caveats.
- Given a Python helper export, the exported records preserve required PeTTa atom fields.

How this changes the spec: it would make it harder for a future revision to replace the PeTTa-native record discipline with a faster host-only prototype while still claiming continuity with the original research intent.

### 3. ThreadKeeper / Subagent Delegation and Records

Surface spec: harden subagent dispatch with path sandboxing, fail-closed budget fallback, disabled/allowlisted shell, timeouts, bounded history, structured returns, persistent transcripts, queue records, cancellation, quotas, patch proposal mode, and audit index verification.

Hidden commitments:

- Delegation should be bounded agency, not uncontrolled autonomy.
- Parent/operator oversight remains central even when child agents do useful work.
- Long-context compression must not erase accountability; summaries need transcript pointers and checksums.
- Patch proposal mode expresses a governance boundary: child agents can suggest changes without mutating trusted state.
- Queue records are not just implementation details; they are accountability artifacts.

Possible Plain addition:

```plain
***philosophical commitments***

- :PhilosophicalCommitment: PC-001 — Subagents are bounded delegates, not independent actors.
  Rationale: A child agent may do useful work, but its authority must be constrained by explicit task contracts, allowed paths, tool quotas, cancellation, and parent/operator review.
  Protects: :SafetyBoundary:, :ExternalAction:, :AgentState:, task contracts, queue records.
  Design implications:
  - Missing or malformed task contracts should fail closed for high-risk actions.
  - File and shell tools must remain sandboxed or explicitly allowlisted.
  - Cancellation and quotas must survive queued dispatch.
  Regression risk: A convenience change that lets queued workers infer missing permissions from prompt text would violate bounded delegation.

- :PhilosophicalCommitment: PC-002 — Summaries are not substitutes for audit trails.
  Rationale: Parent agents need compact returns, but later humans and agents must be able to inspect the underlying transcript and verify integrity.
  Design implications:
  - Structured returns should include transcript paths/checksums where applicable.
  - Hash-chained run indexes and result sidecars should be retained rather than overwritten for convenience.
  Regression risk: Deleting failed claim records or returning summary-only results would make failures easier to ignore and harder to audit.

- :PhilosophicalCommitment: PC-003 — Patch proposal is a permission boundary.
  Rationale: In review-only modes, the child agent's job is to propose, not to mutate.
  Design implications:
  - `patch_proposal_only` must prevent workspace modification even when the proposed patch is valid.
  - Applying a proposed patch requires a separate parent/operator action.
  Regression risk: Auto-applying child patches after successful tests would improve throughput but erase the oversight boundary.
```

Acceptance hooks:

- Given `patch_proposal_only`, child write attempts are captured as proposed changes and do not modify workspace files.
- Given malformed queued task contract fields, worker execution fails before LLM/tool calls.
- Given a completed or failed queued task, retained sidecars and transcript/index entries remain inspectable.
- Given cancellation file appears after enqueue, queued worker preserves and respects it before worker LLM call.
- Given a summary return, transcript path/checksum are recorded where required.

How this changes the spec: it would explicitly identify why ThreadKeeper hardening is not just reliability work. It encodes a theory of accountable delegation and live oversight.

## Proposed Edits to Existing Skill Text

### Risk Tiers

Add to Tier 2 minimum spec requirements:

```markdown
- explicit design rationale or philosophical commitments when the change affects memory, permissions, attribution, model routing, persistent state, inter-agent delegation, or oversight.
```

Add to Tier 3 minimum spec requirements:

```markdown
- explicit design rationale or philosophical commitments when the experiment's representation, evidence records, controls, or substrate choice are part of the research claim.
```

### Step 3 — Create or Update Plain Spec

Change the section list to:

```plain
***definitions***

***design rationale***        # optional; recommended for Tier 2/3 when design intent is easy to lose
# or
***philosophical commitments*** # optional equivalent for explicit commitments

***implementation reqs***

***test reqs***

***functional specs***
```

Add:

```markdown
Use the rationale/commitments section only when it constrains implementation or future revision. Do not add abstract commentary that has no acceptance hook, invariant, non-goal, or review consequence.
```

### Step 4 — Local Plain Lint

Add bullets:

```markdown
- Tier 2/3 specs that affect memory, identity, attribution, permissions, oversight, delegation, cost governance, scientific evidence, or ontology include a `***design rationale***` / `***philosophical commitments***` section, or the review notes explicitly say no material hidden commitment was found;
- each `:PhilosophicalCommitment:` has at least one practical implication: linked concept, requirement, test, invariant, non-goal, or regression risk;
- rationale bullets do not contradict functional specs or implementation requirements.
```

### Intensive Review Prompt

Add review pass:

```text
PASS 6A — Concealed Philosophy / Design Rationale Audit
Check whether the spec hides commitments about identity, memory, attribution, permissions, live oversight, agency boundaries, epistemic trust, scientific evidence, interpretability, governance, substrate choice, or ontology.
For each material commitment, identify where it is implied, how a future implementation could satisfy surface behavior while violating it, and the smallest Plain addition or acceptance hook that should protect it. Do not add abstract commentary unless it changes implementation, tests, architecture, or review gate.
```

Add category `Philosophical-commitment` to issue categories.

Add output section:

```markdown
# Concealed Commitments and Rationale
List material hidden commitments, if any. For each: commitment, implied-by, practical consequence, regression risk, and recommended Plain addition. If none, write `None material for this spec`.
```

### Review Gate Policy

Add under implementation blocked only for serious cases:

```markdown
Implementation may be blocked when a Tier 2/3 spec has an unacknowledged philosophical commitment whose omission would likely cause unsafe authority expansion, persistent-state corruption, loss of auditability, invalid research claims, or reversal of an explicit project-level governance boundary.
```

Add under research conclusions blocked:

```markdown
- the spec's evidence-record or substrate commitments are violated, e.g. a claim depends on inspectable/replayable symbolic records but only unstructured summaries are produced.
```

## Severity Guidance

Use `Philosophical-commitment` issues sparingly.

- **Critical:** hidden commitment affects authority, destructive action, persistent state integrity, user oversight, or validity of required safety gates.
- **Major:** hidden commitment affects architecture, future compatibility, auditability, or a project-level design intent that could easily be reversed.
- **Research-risk:** hidden commitment affects what conclusions can be drawn, especially evidence, substrate, controls, or interpretation.
- **Minor:** rationale would be useful for future maintainers but does not materially affect current implementation.

The reviewer should not produce Critical issues merely because a spec lacks philosophical prose.

## Non-Goals

This extension should not:

- require philosophy sections for every Plain file;
- turn review into broad metaphysical debate;
- block tiny helpers because they lack deep rationale;
- assert that every implementation detail is a philosophical commitment;
- replace ordinary requirements, tests, invariants, or safety boundaries;
- make commitments unchangeable dogma.

A commitment can be revised, but revision should be explicit. The value is preventing accidental reversal.

## Minimal Adoption Plan

1. Add `:PhilosophicalCommitment:` to reusable concepts.
2. Add optional `***design rationale***` / `***philosophical commitments***` to templates and Step 3 guidance.
3. Add the concealed-philosophy pass to the intensive review prompt.
4. Add the three simple lint bullets for Tier 2/3.
5. Try it first on three specs:
   - intent-model-router / Fable cost governance;
   - petta-chem run-contract and symbolic-substrate discipline;
   - ThreadKeeper bounded delegation and audit trails.
6. After one or two real reviews, decide whether the explicit review output section is worth keeping or whether issue-table rows are enough.

## Recommended Final Shape

The best first version is deliberately modest:

- optional section;
- one reusable concept;
- Tier 2/3 reviewer pass;
- practical acceptance hooks;
- no rigid parser requirement;
- no automatic blocking except for safety, governance, persistent-state, or research-validity reversals.

This preserves the Plain Spec Governor's core style: plain, concrete, test-oriented, and useful before code is written.