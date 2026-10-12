# ExplainGate hosted test record

Recorded 12 October 2026; screenshot clock times are Africa/Lagos (UTC+1).

**Status: six demo outcomes and registered learner completion observed.** Evidence consists of user-supplied Studio screenshots and a pasted historical assessment, not independently retrieved chain receipts. This filename is retained for compatibility with the original package links. Expected-only checks are explicitly separated below.

## Deployment and identities

| Field | Observed value |
| --- | --- |
| Studio | https://studio.genlayer.com/ |
| Contract | `0x9897659E227013462b251C507580D1a1BA95687A` |
| Reported version | `explaingate-0.2` |
| Read state | Accepted |
| Author | `0xb32569c8822e66fae73b8eb3a11e0e6034bb4638` |
| Learner | `0x507De2c3391F085e99208C28ad4EEAf567995B43` |
| Course | `0` — Cedar puzzle club practice |
| Course hash | `eede04812324caa34b907422c73ae907fc42d50a25e5dc9a12d40397b7df6d38` |
| Local source SHA-256 | `83b38c586c77ec663bb6189c507841278d1fda39c398d3f5bbd66dc8076b5a06` |

The deployment transaction hash and full successful consensus receipts are not recorded here. The local source hash identifies the supplied file; deployed byte equality was not independently verified. Writes shown in screenshots had Simulation Mode unchecked. Execution-mode metadata was not independently fetched. Earlier v0.1 observations belong to the separate address in CASE3_FIX.md.

## Six saved previews

Actual successful save order was **3, 1, 2, 4, 5, 6**. Demo case IDs and saved result IDs are different counters.

| Case ID | Saved result ID | Observed decision | Criterion statuses | Whole-answer extra | Readiness / path | Screenshot time |
| --- | --- | --- | --- | --- | --- | --- |
| 3 | 0 | RETRY | C1 MET; C2 MET; C3 INCORRECT | CONTRADICTS_REFERENCE | READY / ANSWER_REVIEW | 10:16 |
| 1 | 1 | PASSED | C1 MET; C2 MET; C3 MET | NONE | READY / ANSWER_REVIEW | 10:28 |
| 2 | 2 | RETRY | C1 MET; C2 MISSING; C3 MET | NONE | READY / ANSWER_REVIEW | 10:32 |
| 4 | 3 | RETRY | C1 MET; C2 MET; C3 MET | CONTRADICTS_REFERENCE | READY / ANSWER_REVIEW | 10:34 |
| 5 | 4 | RETRY | C1 MET; C2 MET; C3 MET | INSTRUCTION_ATTEMPT | READY / ANSWER_REVIEW | 10:39 |
| 6 | 5 | REVIEW_REQUIRED | C1/C2/C3 NOT_ASSESSED | NOT_ASSESSED | CONFLICTING / CHECKPOINT_REVIEW | 10:41 |

Every summary showed `input_kind: FICTIONAL_DEMO_PREVIEW` and `progress_advanced: false`. At 10:46, get_counts showed attempts 0, courses 0, demo_cases 6, demo_results 6. These saved outcomes do not imply every submission succeeded: case 1 first produced a blank-reason error, then succeeded on a manual retry.

## Registered learner walkthrough

| Step | Evidence | Observed outcome |
| --- | --- | --- |
| Create course | 10:50 get_course(0) | Two checkpoints; expected author, hash and material; open true |
| Read initial learner state | 10:58 get_progress | completed_steps 0, next_checkpoint 0, total_steps 2, can_attempt true |
| Attempt checkpoint 1 too early | 11:03 transaction error | FINALIZED / ERROR: `Submit only your next checkpoint; refresh progress.` |
| Submit incomplete checkpoint 0 | 11:08 get_attempt_summary(0) | READY; C1 MET, C2 MISSING, C3 MET; extra NONE; RETRY; progress 0 to 0; advanced false |
| Submit complete checkpoint 0 | 11:36 get_attempt_summary(1), refreshed reads at 11:49 | READY; C1/C2/C3 MET; extra NONE; PASSED; progress 0 to 1; advanced true |
| Recover imported existing deployment | 12:26 get_progress | Course 0 and progress 1 retained; next_checkpoint 1; course_open true |
| Submit complete checkpoint 1, with manual retry | 12:41 get_progress | completed_steps 2, total_steps 2, course_complete true, next_checkpoint -1, can_attempt false, course_open true |

Attempt 0 hash: `409b3a9ec3eb356f8b314096028053851c32a88c8f968bcc3905b041384aeae5`.

Attempt 1 hash: `8b431cde226795136a1a24168fca83cf28469dae593faf5c1741082ab612b07a`.

The last observed counts before the second checkpoint were attempts 2, courses 1, demo_cases 6, demo_results 6 (11:49 and 11:57). Final counts were not supplied. The second checkpoint's attempt summary, ID, hash and successful transaction receipt were not supplied; its completion is supported by the final Accepted-state progress read. Under the supplied source's rules, completing that step requires a PASSED assessment; the individual assessment fields are not separately claimed as observed.

The final progress response is transcribed in [HOSTED_PROGRESS.json](HOSTED_PROGRESS.json). Screenshot reference: `Screenshot_2026-10-12-12-41-07-803_com.android.chrome.jpg`. Images were reviewed in the conversation and are not bundled in this package.

## Failed submissions retained in the record

| Time | Operation | Observed failure | Later observation |
| --- | --- | --- | --- |
| 10:20 | run_demo(1) | Blank readiness.reason rejected by _check_readiness; `ValueError: Text must not be blank.`; execution error with an undetermined consensus history | Case 1 stored as result 1 at 10:28 after manual retry |
| 11:14 | submit_answer, checkpoint 0, complete answer | UNDETERMINED / ERROR; _check_report rejected blank extra.reason with the same ValueError | Attempt 1 later showed PASSED; progress advanced to 1 |
| 12:32 | submit_answer, checkpoint 1, complete answer | FINALIZED / ERROR; _check_readiness rejected blank reason with the same ValueError | A subsequent 12:41 progress read showed completion at 2 |

These are evaluator-output validation failures, distinct from educational RETRY decisions. The submitted answers were nonblank. Tracebacks reached model-output validation, so malformed punctuation in manually copied diagnostic input text is not established as their cause.

No source change added automatic repair or retries. The validators and schema checks were not bypassed. Manual retry success is not a reliability fix or evidence of a measured success rate. The original code rejects malformed output before appending an attempt or advancing progress. Stored-state observations and code checks are distinct from an independent audit of every failed transaction.

## Deferred live checks and limitations

- Replay of an already passed checkpoint: not conclusively evidenced by a transaction error screenshot. Local fixture coverage only.
- Cross-wallet progress isolation: not separately demonstrated by a second live progress read. Local fixture coverage only.
- Author-only closure, blocked post-closure attempts and retained history: not run to completion in this hosted walkthrough. Local fixture coverage only. The last course state is still open.
- Finality of all successful transactions, all validator reports and exact network/runner metadata: not independently retrieved.
- Model grading quality, prompt-injection robustness, latency, cost and broad reliability: not established by this small exercise.

The manual learner walkthrough ends here. These deferred checks must not be counted as hosted passes or used to imply production readiness.
