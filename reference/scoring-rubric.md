# Engineering Council Scoring Rubric

Score each dimension 0–10. Use evidence, not model identity.

| Dimension | Weight | What good looks like |
|---|---:|---|
| Correctness | 20 | Satisfies requirements; invariants are explicit; edge cases handled |
| Simplicity | 10 | Few moving parts; understandable control/data flow |
| Reliability | 15 | Graceful failure, retries/failsafes where appropriate, recoverable state |
| Security & privacy | 10 | Least privilege, safe inputs, no secret leakage, threat-aware design |
| Maintainability | 10 | Clear ownership, modular interfaces, low coupling, good docs |
| Testability | 10 | Deterministic seams, useful unit/integration/e2e tests |
| Performance | 10 | Meets latency/throughput/resource constraints with evidence |
| Scalability | 5 | Handles plausible future growth without premature complexity |
| Compatibility | 5 | Fits repository conventions and existing interfaces |
| Migration/rollback | 5 | Safe adoption path and realistic rollback |

## Veto conditions

A proposal can be rejected regardless of weighted score for:

- known critical security flaw,
- unrecoverable data corruption risk,
- violation of a hard user requirement,
- unsafe behavior in a safety-critical control path,
- dependency on unavailable/forbidden infrastructure,
- no credible migration or rollback for a destructive change.

## Decision guidance

Prefer the lowest-complexity design that satisfies hard constraints and has an adequate safety/reliability margin. Do not reward novelty for its own sake.
