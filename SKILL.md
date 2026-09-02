---
name: ai-engineering-council
description: Run a no-paid-API, browser-based multi-model engineering debate before major implementation work. Use when the user asks for a council, debate, second opinions, architecture review, adversarial review, or when a task involves a major architecture change, algorithm choice, data migration, security-sensitive subsystem, reliability-critical design, or other nontrivial implementation where independent proposals and critiques would materially reduce risk. Coordinate signed-in web chat applications such as Claude, Grok, and Gemini when browser/computer-use access is available; keep proposals independent, anonymize cross-critiques, score alternatives, write a decision record, and only then implement and test.
---

# AI Engineering Council

Run a structured engineering council before implementing consequential changes. Prefer independent disagreement over superficial consensus.

## Non-negotiable rules

1. Do not use paid model APIs or request API keys for the council workflow.
2. Use browser/web chat interfaces only when the environment provides browser/computer-use access and the user is already authorized to use those services.
3. Never enter, request, reveal, copy, or store passwords, recovery codes, API keys, cookies, session tokens, or other credentials.
4. Never bypass CAPTCHA, anti-bot checks, rate limits, access controls, subscription gates, or provider restrictions. If a site blocks automation, stop using that provider and record the limitation.
5. Do not upload the entire repository to external chats. Share the minimum context required. Redact secrets, personal data, private keys, tokens, internal URLs, customer data, and unrelated proprietary code.
6. Keep the proposal phase independent. Do not show one model another model's proposal until the cross-critique phase.
7. Do not modify production code during the council phase.
8. Do not declare consensus merely because most models agree. Optimize for technical quality against explicit criteria.
9. Preserve raw council artifacts under `.council/` so the decision is auditable.
10. If external agents are unavailable and the task explicitly requires an external council, do not pretend the debate occurred. Record the limitation and stop at the implementation gate unless the user explicitly permits a local-only fallback.

## Default council members

Treat provider names as roles, not guarantees of availability:

- Codex: repository expert and implementation owner.
- Claude web: architecture reviewer.
- Grok web: adversarial reviewer / devil's advocate.
- Gemini web: alternative-design and research reviewer.

If one provider is unavailable, continue with the remaining independent reviewers. Prefer at least two external perspectives for `full` mode when possible.

## Council modes

Choose a mode automatically unless the user specifies one.

### Lite

Use for medium-risk features, refactors, or algorithm choices with limited blast radius.

- 2 independent proposals total, including Codex.
- 1 critique round.
- Short scorecard.
- Post-implementation review optional unless tests are weak.

### Full

Use for architecture changes, data models, distributed systems, robotics/autonomy logic, ML pipelines, auth/security, migrations, production infrastructure, or changes with significant blast radius.

- Codex + up to 3 external independent proposals.
- Anonymous cross-critique.
- Rebuttal round.
- Dedicated red-team pass.
- Weighted scorecard and ADR-style decision.
- Mandatory post-implementation review.

### Red-team

Use when the user primarily asks to attack an existing plan or implementation.

- Focus on failure discovery, hidden assumptions, abuse cases, reliability, security, performance, and rollback.
- Do not redesign unless a discovered issue requires it.

## Workflow

### Phase 0 — Inspect before asking outsiders

Inspect the repository locally first. Identify:

- affected modules and interfaces,
- existing architectural patterns,
- tests and validation commands,
- constraints from README/AGENTS/project docs,
- likely sensitive material that must not be shared externally.

Create a council run directory using `scripts/init_council.py` when available.

### Phase 1 — Freeze the specification

Write `.council/<run>/00-specification.md` using `templates/specification.md`.

Include:

- objective,
- requirements,
- non-goals,
- constraints,
- current architecture relevant to the change,
- success criteria,
- edge cases,
- security/privacy constraints,
- performance/reliability constraints,
- files/modules likely affected,
- questions the council must resolve.

Do not ask models to debate an ambiguous one-line task when local repository inspection can resolve the ambiguity.

### Phase 2 — Independent proposals

Ask each available agent the same frozen specification independently. Do not reveal the identities or answers of other agents.

Use `templates/proposal.md` as the response contract.

Require each proposal to address:

- architecture,
- algorithm/data structures,
- data/control flow,
- interfaces,
- dependencies,
- failure modes,
- security/privacy,
- performance,
- migration/rollback,
- testing,
- reasons not to choose the proposal.

Save raw responses separately:

- `10-codex-proposal.md`
- `11-external-a-proposal.md`
- `12-external-b-proposal.md`
- `13-external-c-proposal.md`

Maintain `providers.md` mapping A/B/C to the actual provider. Do not expose that mapping to reviewers during anonymous critique.

### Phase 3 — Anonymous cross-critique

Normalize proposals as A/B/C/D and remove provider-identifying wording when practical.

Ask reviewers to critique all proposals using `templates/critique.md`.

Prioritize:

- incorrect assumptions,
- hidden coupling,
- unnecessary complexity,
- concurrency/race issues,
- security flaws,
- scalability bottlenecks,
- operational burden,
- maintainability,
- weak observability,
- missing tests,
- rollback difficulty,
- simpler alternatives.

Save critiques under `20-*` files.

### Phase 4 — Rebuttal and revision

For Full mode, give each proposal the strongest objections against it. Allow the proposal to:

- defend the original design,
- revise it,
- merge a stronger idea,
- concede that another design is better.

Save revised proposals under `30-*` files.

### Phase 5 — Red-team pass

Perform a dedicated adversarial pass independent of popularity. Ask:

- How can this fail in production?
- What assumption is most likely false?
- What happens under partial failure?
- What happens with malformed or adversarial input?
- What happens at 10x load/data size/device count?
- What happens if a dependency is unavailable?
- What state can become corrupted or unrecoverable?
- Which tests could pass while the implementation is still wrong?
- Is there a simpler design with fewer moving parts?

For robotics/autonomy, also consider stale sensors, timing jitter, control-loop latency, communication loss, unsafe commands, failsafe behavior, and simulation-to-real gaps.

### Phase 6 — Judge against a rubric

Use `references/scoring-rubric.md`.

Score proposals without using model identity as evidence. Cite concrete claims from proposals/critiques.

The winner may be:

- one proposal unchanged,
- one proposal with revisions,
- a hybrid design,
- "no proposal is safe enough yet".

Do not average away a catastrophic flaw. A severe security, correctness, or recoverability issue can veto a high aggregate score.

### Phase 7 — Implementation gate

Write `.council/<run>/40-decision.md` using `templates/decision.md` before editing implementation files.

The decision must contain:

- chosen architecture,
- rejected alternatives and why,
- unresolved risks,
- affected files/modules,
- implementation sequence,
- test plan,
- observability plan where relevant,
- migration/rollback plan,
- explicit implementation gate: `APPROVED`, `APPROVED_WITH_CONDITIONS`, or `BLOCKED`.

If `BLOCKED`, do not implement the disputed change.

### Phase 8 — Implement incrementally

After approval:

1. Implement the smallest coherent slice.
2. Run focused tests.
3. Run lint/type/static checks relevant to the repository.
4. Continue incrementally.
5. Run broader regression tests before completion.
6. Record deviations from the council decision in `50-implementation-notes.md`.

If implementation reveals an assumption that invalidates the selected architecture, reopen the council instead of silently improvising.

### Phase 9 — Post-implementation review

For Full mode, provide the sanitized diff/summary and test results to at least one independent external reviewer when available.

Use `templates/post-review.md`.

Ask the reviewer specifically:

> What is wrong with this implementation even if all current tests pass?

Resolve material findings, rerun tests, and save `60-post-review.md`.

## Browser interaction protocol

Read `references/browser-agents.md` before using external web chat applications.

When browser/computer-use is available:

1. Reuse already-open, signed-in tabs where possible.
2. Navigate only to the intended provider site.
3. If authentication is required, stop and let the user authenticate; do not handle credentials.
4. Start a fresh conversation for each independent proposal when feasible.
5. Paste only the sanitized specification/context.
6. Capture the complete relevant answer in the council run directory.
7. Watch for truncation, failed sends, rate limits, stale drafts, or answers from the wrong tab.
8. Verify each saved answer corresponds to the intended provider and round.

If a provider UI changes, adapt conservatively. Never infer that a send succeeded without checking the visible result.

## Context-sharing policy

Before sending repository context to an external model:

- run a secret scan if the repository already has one,
- inspect `.env*`, credential files, configs, and private keys,
- prefer interface summaries over copying large files,
- include code snippets only when necessary,
- remove usernames, tokens, endpoints, IDs, and personal/customer data,
- keep a note of what was shared in `providers.md`.

If sanitization cannot be done confidently, keep the analysis local.

## Final response format

Report, concisely:

1. Council mode and participating reviewers.
2. The major disagreement(s).
3. Chosen architecture and why.
4. Important rejected alternatives.
5. Implementation/test status.
6. Remaining risks or blocked items.
7. Path to the council decision artifact.

Do not dump all model transcripts into the final response unless requested.
