# ExplainGate

ExplainGate is a GenLayer Intelligent Contract for short learning checkpoints. A course author fixes the reference material, question and assessment criteria. A learner submits an explanation in their own words. An accepted `PASSED` assessment advances that wallet by exactly one checkpoint; other decisions preserve its current position.

**Version 0.2, 12 October 2026:** 83 fixture-based local tests, SDK lint and validation passed. User-supplied GenLayer Studio Accepted-state screenshots show all six demo outcomes and a registered learner completing both checkpoints. Some submissions failed when a model returned a blank reason; manual retries later succeeded. This remains an unresolved reliability limitation. See [VERIFICATION.md](VERIFICATION.md) and the [hosted test record](DEMO_RESULTS_TEMPLATE.md).

Observed version-0.2 deployment: `0x9897659E227013462b251C507580D1a1BA95687A` on [GenLayer Studio](https://studio.genlayer.com/). Course `0`, learner `0x507De2c3391F085e99208C28ad4EEAf567995B43`: `completed_steps: 2`, `course_complete: true`, `next_checkpoint: -1`. This is screenshot-based evidence, not an independently retrieved finalized receipt.

The earlier version-0.1 case-3 diagnosis and rubric correction are retained in [CASE3_FIX.md](CASE3_FIX.md). Existing users can import the deployed address to recover access; importing source alone does not restore the deployment association. There is no need to redeploy or repeat completed tests to use this documentation update.

Start with [START_HERE.md](START_HERE.md). The [demo guide](DEMO_GUIDE.md) contains reproduction steps for six previews and a separate two-account registered workflow. See [verification](VERIFICATION.md), [design research](IDEA_RESEARCH.md) and the completed [hosted test record](DEMO_RESULTS_TEMPLATE.md), which distinguishes observed and deferred checks.

## Example

A fictional reading club explains how volunteers should handle puzzle boxes. The first checkpoint asks a learner to explain the borrowing limit, why a checkout code is recorded, and where the box is returned. An answer can use different words, but it must convey all three ideas. Passing unlocks a second checkpoint about resetting the puzzle, counting its pieces and recording a missing piece.

This focuses on the content of a submitted practice answer. It does not establish who wrote it, whether help was used, or whether the person can perform the activity.

## Decisions and consequences

| Decision | Meaning | Registered progress |
| --- | --- | --- |
| `PASSED` | Ready checkpoint; all criteria MET; no contradiction, manipulation or unresolved issue anywhere in the answer | Advance exactly one checkpoint |
| `RETRY` | A criterion is MISSING/INCORRECT, or the answer contains a contradiction or evaluator instruction | Keep current checkpoint; preserve attempt |
| `REVIEW_REQUIRED` | Checkpoint is conflicting/unclear, or an answer dimension remains unclear | Keep current checkpoint; preserve evidence |

A demo result always has `progress_advanced: false`, even when its decision is PASSED. Demos and registered attempts use separate storage and separate IDs.

## Consensus design

1. The leader evaluates **checkpoint readiness without the learner answer**. The reference, question and every criterion must be interpretable, consistent and answerable from the stipulated reference.
2. An unready checkpoint goes directly to `CHECKPOINT_REVIEW`. Code constructs NOT_ASSESSED answer fields; the learner is not graded against contradictory or unsupported material.
3. For a ready checkpoint, the leader evaluates the full answer against each criterion. The field named `extra` checks the entire answer for contradictions and evaluator instructions, including statements already covered by a criterion. A wrong return shelf is both C3 INCORRECT and extra CONTRADICTS_REFERENCE. Partial keyword matches, omitted reasons and later contradictions do not justify a pass.
4. Each validator independently repeats the assessment without seeing the leader's proposal. Every criterion status, the readiness status and the whole-answer status must match. Matching only the final decision is insufficient.
5. A separate validator audit checks the proposed explanations and quotations against the complete supplied material. Quotes and reasons may differ between independent assessments, but proposed evidence must survive this substantive audit.
6. Deterministic code validates schemas, IDs and verbatim quotations, derives the decision, and changes progress only after the returned assessment passes those checks.

Source/answer content is explicitly treated as data, including attempted instructions to the evaluator. This design and its synthetic attack fixture are not a guarantee of prompt-injection resistance. Substring checks alone do not establish correct interpretation.

The contract stores the accepted leader assessment. It does not store every validator's private intermediate report. A consensus failure is not a saved REVIEW_REQUIRED result: inspect execution/consensus status and persisted state before calling a hosted attempt successful.

## Course and learner identity

- A course has 1–3 ordered checkpoints, each with 1–4 criteria. Criteria are sorted by their IDs; checkpoint order is meaningful and preserved.
- Course content has no editing method. The course hash binds contract address, author wallet, course ID, title, checkpoints and rubric version.
- A registered submission supplies the expected course hash and checkpoint index. It may target only that wallet's next checkpoint. A successful repeated/stale submission cannot credit the same step twice.
- Progress is separate for every course ID and learner wallet. An author must use a different learner wallet for the registered workflow. Separate addresses do not prove separate people.
- Only the author can close a course. Closure is terminal and stops new submissions; it preserves prior attempts, course content, hashes and completed steps. It does not revoke historical completion.
- `course_complete: true` means this wallet has passed all checkpoints under this course's rules. It is not a certificate or verified competence claim.

## Public methods

| Method | Type | Use |
| --- | --- | --- |
| `get_version()` | Read | Returns `explaingate-0.2` |
| `get_counts()` | Read | Courses, registered attempts, saved demos and six available cases |
| `create_course(title, checkpoints_json)` | Write | Freeze course material; caller becomes author |
| `get_course(course_id)` | Read | Material, author, course hash and current open state |
| `get_progress(course_id, learner_address)` | Read | Completed steps, next checkpoint and current ability to submit |
| `submit_answer(course_id, expected_course_hash, checkpoint_index, answer)` | Write | Assess the submitting wallet's next checkpoint |
| `get_attempt(attempt_id)` | Read | Full immutable registered attempt and assessment |
| `get_attempt_summary(attempt_id)` | Read | Decision, criterion statuses and recorded progress transition |
| `close_course(course_id, expected_course_hash)` | Write | Author stops new attempts |
| `get_demo_course()` | Read | Example two-checkpoint course JSON |
| `get_demo(case_id)` | Read | Inspect a fictional preview input |
| `run_demo(case_id)` | Write | Save a preview assessment without registered progress |
| `get_demo_result(result_id)` | Read | Full stored preview |
| `get_demo_summary(result_id)` | Read | Concise stored preview |

Course IDs, registered attempt IDs, demo result IDs and checkpoint indexes each start at **0**. Demo **case IDs start at 1**. These numbers are not interchangeable.

## Bounds and intended scope

English, low-stakes practice exercises only. Reference text: 1,800 characters per checkpoint; question: 500; criterion: 300; answer: 3,000; quoted evidence: 800; explanation: 400. Course title: 120; checkpoint title: 80. The course JSON input accepts up to 160,000 characters to accommodate escaped Unicode; structural field bounds still apply.

There is no web retrieval, automatic content correction, payment, NFT, certification, credential verification, anti-cheating system, private storage or external learning-platform integration. Answers and course content are public. Do not put private learner information in the prototype. Course authors are responsible for the accuracy and appropriateness of their material; reference truth is not independently verified.

No total record quota is imposed. Per-call bounds do not establish sustainable storage/cost at scale. Attempts can be repeated until passed, so completion does not measure independent mastery. Grading quality, model agreement rate, latency, cost, accessibility and language robustness need broader evaluation. The current code strictly rejects blank model reasons and has no automatic retry or repair. The observed manual retry successes do not fix that limitation or establish a success rate. No native upgrader or state-preserving upgrade workflow is configured; a future code revision requires a separately assessed deployment plan. Importing the current address recovers access to that existing instance.

## Local reproduction

Use Python 3.12 from this directory:

```bash
python3.12 -m venv .venv
.venv/bin/python -m pip install -r requirements-dev.txt
.venv/bin/python prepare_test_sdk.py
.venv/bin/python -m pytest test_explaingate.py -q
GENVM_VERSION=v0.3.0-rc7 .venv/bin/genvm-lint check explaingate.py
```

The preparation script uses the official release lookup and shares the downloaded archive with direct-mode tests. The test package's built-in unprepared download URL returns 404 for this release, so run the preparation step first. Dependency versions are pinned for reproduction, not claimed to be the latest.

The package uses a **flat file layout** so selecting all files on an Android phone preserves test and documentation paths during GitHub upload.

Repository description: GenLayer learning checkpoints that assess explanations against fixed criteria and advance wallet-specific course progress.
