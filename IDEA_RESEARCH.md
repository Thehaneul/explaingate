# ExplainGate concept selection

Research date: 12 October 2026. This is a bounded design investigation, not a market-demand study or exhaustive novelty search.

## Three concepts compared

| Concept | Shared decision and state change | Existing overlap / tradeoff | Choice |
| --- | --- | --- | --- |
| Semantic duplicate-question registry | Decide whether differently worded questions belong to one registered issue | [DedupRegistry](https://github.com/Ritapossible/GenLayer-Dedup-Registry) already implements a semantic registry and describes bug reports and governance proposals; keeping merges fair adds identity and clustering issues | Not selected |
| Contribution review | Decide whether submitted work meets a program's rubric, recording an approval | [Contribution Judge](https://github.com/minhhnob/genlayer-contribution-judge) already reviews campaign evidence; [ProofPilot](https://github.com/Manablaq/proofpilot) describes builder-evidence reviews | Not selected |
| ExplainGate learning checkpoints | Decide whether a free-text answer meets fixed learning criteria, then advance only the submitting wallet's next checkpoint | [GenQuiz](https://www.genquiz.online/) already offers competitive GenLayer quizzes; this concept instead focuses on frozen short-answer criteria, quoted assessment evidence and sequential progress | Selected |

The narrower learning workflow gives this project a different application from our translation-fidelity NoticeParity, work-scope ScopeHandshake, dataset-overlap HoldoutLedger and provenance SourceRoots. Education and AI assessment are established categories; no claim is made to have invented them. Limited exact-name searches did not establish an existing GenLayer project named ExplainGate. This does not establish trademark availability or global uniqueness. ExplainGate is a working name.

## Why GenLayer can fit

[GenLayer's fit guide](https://docs.genlayer.com/developers/intelligent-contracts/when-to-use-genlayer) asks for a shared judgment backed by common evidence and a concrete state consequence. Here:

- Decision: did one exact answer satisfy every fixed criterion under the exercise's reference?
- Consequence: advance the submitting wallet by one ordered checkpoint, or preserve its current position.
- Evidence: immutable course content plus the exact answer; no changing website or private evaluator backend.
- Exact agreement: readiness status, every criterion's status and extra-content status. Explanatory wording may differ.
- Failure: model/consensus failure produces no accepted progress update; accepted uncertainty becomes a review hold.

The [equivalence-principle documentation](https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle) requires substantive independent verification rather than schema-only acceptance. ExplainGate repeats the assessment independently and adds an audit of the leader's cited evidence. It derives the final decision and progress transition in deterministic code.

The blockchain component is useful only if multiple parties need to inspect and rely on the same progress record. A private tutoring tool without that requirement could use a conventional backend. No external course provider currently consumes ExplainGate progress.

## Design choices

- A separate readiness check precedes answer assessment. Conflicting course material leads to NOT_ASSESSED answer fields, instead of forcing models to agree on unnecessary grades for a broken exercise.
- Checkpoints advance in order, and the request includes the expected index. This makes a repeated successful submission fail rather than silently applying it to the next question.
- Course authors cannot submit as the same wallet. This supports a clear two-role demo, but does not establish identity or prevent one person controlling multiple accounts.
- Demo previews are isolated from real progress. No preview can unlock a registered step.
- Closing a course prevents new attempts while preserving history. A corrected course must be published as a new course with new identity and progress.
- Fictional club instructions keep the initial examples low stakes and self-contained. There is no professional qualification, exam proctoring, rewards or payment mechanism.

## Evidence still needed

Hosted deployment, real-model agreement for all six cases, the two-account workflow, repeated paraphrase/misconception trials, misleading or adversarial answers, accessibility and language behavior, cost/latency measurements, and actual demand from course providers. The 79 local tests use authored model responses and do not answer those research questions.

Other-project descriptions above are based on their published project pages. Their deployment, security or performance was not independently tested.
