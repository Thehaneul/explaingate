# ExplainGate verification

Verification date: 12 October 2026.

- Source file: `explaingate.py`; version `explaingate-0.2`.
- Source SHA-256: `83b38c586c77ec663bb6189c507841278d1fda39c398d3f5bbd66dc8076b5a06`.
- Python: 3.12.14.
- Tool versions: genlayer-test 0.29.2; genvm-linter 0.11.0; pytest 9.1.1; cloudpickle 3.1.2.

**Local result: 83 tests passed. SDK lint (3 checks) and validation passed.** The public schema contains 14 methods: 10 reads and 4 writes. This is local verification using authored model-response fixtures, not a hosted-model or network-consensus result.

Version-0.2 hosted status: **all six demo outcomes and two-checkpoint learner completion observed in user-supplied Accepted-state screenshots** on 12 October 2026. Contract: `0x9897659E227013462b251C507580D1a1BA95687A`. The final 12:41 screenshot shows completed_steps 2, course_complete true, next_checkpoint -1 and can_attempt false for course 0 and the documented learner. See [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md) for the evidence record and [HOSTED_PROGRESS.json](HOSTED_PROGRESS.json) for the final progress transcription.

These are user-operated Studio observations. No deployment receipt, complete validator reports or final successful checkpoint-1 transaction receipt was independently retrieved. The deployed source bytes were not independently compared with the local source hash. Final checkpoint completion is evidenced by the progress read; its attempt ID, report, hash and final total attempt count were not supplied.

**Known reliability issue:** blank model-generated reasons caused execution errors during the case-1 demo, registered checkpoint 0 and registered checkpoint 1. Successful manual retries followed. Source code was not changed to add automatic retries or tolerate blank reasons. These failures are distinct from saved RETRY decisions and remain part of the evidence. The prototype is not demonstrated to be reliable on the first submission.

The historical version-0.1 case-3 consensus failure and the version-0.2 rubric correction are documented in [CASE3_FIX.md](CASE3_FIX.md).

## Reproduction

See requirements-dev.txt and prepare_test_sdk.py. Python 3.12 is required for this pinned setup. SDK release: v0.3.0-rc7. Source dependency: `py-genlayer:1jb45aa8ynh2a9c9xn3b7qqh8sm5q93hwfp7jqmwsfhh8jpz09h6`.

Observed commands after preparing the official SDK archive:

```text
python -m pytest test_explaingate.py -q -x
83 passed in 1.80s

GENVM_VERSION=v0.3.0-rc7 genvm-lint check explaingate.py
Lint passed (3 checks)
Validation passed
Contract: ExplainGate
Methods: 14 (10 view, 4 write)
```

An initial run could not set up fixtures because genlayer-test's default release-asset URL returned HTTP 404. Running the supplied prepare_test_sdk.py resolved this through the linter's official release-asset lookup; the subsequent suite completed successfully. No dependency, permission or assertion was bypassed. The linter notes that a newer runner exists; this package retains the explicitly validated pin, also used by the preceding NoticeParity project.

The test plugin prints a default localhost RPC configuration. Direct-mode tests use controlled local model fixtures and do not send those calls to a blockchain endpoint.

## What the tests establish

- All six authored preview inputs produce their documented decisions given the authored reports. Previews never create courses or registered attempts and cannot unlock registered steps.
- A learner can record an incomplete answer without advancing, then pass checkpoint 0 and checkpoint 1 in order. Completion affects only that wallet and course.
- Wrong hashes, author self-submission, skipped/stale checkpoints, invalid indexes and blank/oversized answers fail before model execution or saved attempts.
- A passed checkpoint cannot be credited twice. The course hash binds exact content and identity. Criterion ordering is canonicalized, while checkpoint order is retained.
- Progress cannot be transferred between wallets or courses by reusing IDs or a different course hash.
- Only the author can close the course, with the correct hash. Closure is terminal, blocks submissions and preserves immutable attempts, hashes and recorded progress.
- Invalid course shapes, duplicate JSON keys, duplicate/invalid criterion IDs, excessive counts and invalid reference lengths are rejected before course creation.
- Malformed model output, missing/duplicate/unknown report IDs, invented/empty or status-inconsistent quotes, extra fields, invalid statuses and blank reasons cannot append an attempt or advance progress.
- Checkpoint-readiness evidence must cite named fields verbatim without duplicate evidence. An unready checkpoint's report cannot smuggle graded criteria into the NOT_ASSESSED path.
- Validators independently compare readiness, each criterion and whole-answer status, including cases where different criterion classifications would still yield the same final RETRY decision.
- Different explanations and row order are acceptable when substantive classifications agree and the evidence audit accepts. Every evidence audit must return the exact object with boolean true; false, string/integer lookalikes or extra fields are rejected on both assessment paths.
- Failed or malformed leader results are rejected. Uncertainty takes priority over a known incorrect answer.
- A maximum-size non-BMP Unicode input round-trips through escaped JSON storage on the unready-checkpoint path without truncating the answer. It is a boundary fixture, not evidence that non-English exercises are supported.
- Both captured leader and validator functions serialize through the SDK's Python cloudpickle check, with RuntimeWarnings treated as errors.

## Case-3 regression checks added in version 0.2

The exact assessment pasted by the user is retained in case3_v01_observed_report.json. Two tests exercise it as the leader report in preview and registered paths, use independently supplied fixture wording for validation, and assert RETRY without progress. Two tests confirm that differing whole-answer classifications and a negative evidence audit still cause validator rejection. Model generation and the evidence audit are mocked; these tests verify code handling, not improved hosted-model agreement.

## What remains unverified

Authored fixtures do not measure answer quality, prompt-injection robustness, educational validity, fairness, real-model agreement, transaction finality, live GenVM serialization, throughput, latency or cost. In particular, the injection demo tests how code handles an INSTRUCTION_ATTEMPT report; it does not establish that every model will detect the command.

Direct mode runs a leader locally and exposes a captured validator for explicit testing. A false validator result in these tests does not simulate network rollback: locally stored leader state is not proof of an accepted transaction. Network voting, retry/appeal behavior and storage rollback on failed consensus require hosted testing.

No proof of personal identity, independent authorship or practical competence is provided. The course author chooses the reference facts; the contract checks their internal usability, not outside truth. One person can control both author and learner accounts. Completion is specific to the supplied material, rules and wallet.

The hosted walkthrough is complete through learner completion. Additional live replay, wallet-isolation and course-closure checks were deferred; local fixtures cover these behaviors, but they must not be described as live-verified. The course remained open in the last screenshot. No further manual testing is required for this recorded walkthrough. A future reliability revision should address invalid model responses and receive its own verification.
