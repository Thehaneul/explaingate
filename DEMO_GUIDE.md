# ExplainGate Studio walkthrough

This guide describes **expected version-0.2** outcomes for reproduction on a new instance. Completed hosted observations, including manual retries and deferred checks, are recorded separately in [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md). The existing learner walkthrough has finished; it does not need to be repeated. Earlier version-0.1 observations and the case-3 correction are in [CASE3_FIX.md](CASE3_FIX.md). Use fictional content and two Studio accounts for a new registered exercise.

## A. Deploy and identify the instance

Follow [START_HERE.md](START_HERE.md). Deploy `explaingate.py` with no constructor arguments in Normal / Full Consensus mode. Use a new instance and keep Simulation Mode unchecked for writes. Call `get_version`: expected `explaingate-0.2`.

Save the contract address. When checking transactions, inspect execution result, consensus history and resulting stored records. An accepted/finalized error is still a failed operation. If consensus ends undetermined, stop and inspect it; do not pretend a result was saved or blindly repeat all earlier cases.

## B. Six demo previews — one account

Run each case once, in order, and wait for the preceding transaction before sending the next. Then call `get_demo_summary` with its result ID. All previews have `progress_advanced: false` and `input_kind: FICTIONAL_DEMO_PREVIEW`.

| `run_demo` case_id | `get_demo_summary` result_id on a fresh instance | Expected decision | What to check |
| --- | --- | --- | --- |
| 1 | 0 | PASSED | C1/C2/C3 MET; extra NONE; readiness READY |
| 2 | 1 | RETRY | C2 MISSING: no explanation of the checkout log; C1/C3 MET |
| 3 | 2 | RETRY | C3 INCORRECT: red shelf instead of blue; C1/C2 MET; extra CONTRADICTS_REFERENCE |
| 4 | 3 | RETRY | All criteria MET; extra CONTRADICTS_REFERENCE: 08:00 instead of 09:00 |
| 5 | 4 | RETRY | All criteria MET; extra INSTRUCTION_ATTEMPT |
| 6 | 5 | REVIEW_REQUIRED | readiness CONFLICTING; CHECKPOINT_REVIEW; every criterion and extra NOT_ASSESSED |

`get_demo(case_id)` shows the exact input without assessing it. `get_demo_result(result_id)` shows the stored full evidence. Case 6 intentionally gives contradictory borrowing rules: one box versus exactly two at a time. It should record a review hold without grading the answer.

After one successful pass through all cases, `get_counts` should show courses 0, attempts 0, demo_results 6 and demo_cases 6. A retry of a successfully saved demo creates a new result ID. Failed transactions do not create results. If you repeated cases, use get_counts and inspect the full result's case_id; do not assume case_id minus one is still the correct result ID.

Record screenshots and transaction hashes in [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md).

## C. Registered workflow — author A and learner B

The previews did not create a real course. This section demonstrates that an assessment changes progress for a registered learner. You can use two Studio accounts, switching in the account menu. Both may display 0 GEN in Studio; inspect any actual transaction error instead of assuming a balance issue.

### 1. Account A creates the course

Open `create_course`:

- title: `Cedar puzzle club practice`
- checkpoints_json: paste the full contents of [demo_course.json](demo_course.json).

Send the transaction and confirm success. Call `get_course` with course_id **0** on a fresh instance. Copy its **course_hash**, exactly as returned, without quotes or a `0x` prefix. Also copy Account B's full wallet address.

If a course already exists because you repeated this operation, obtain the actual new ID from get_counts and inspect that course. Reuse that same ID and hash throughout this walkthrough.

### 2. Switch to Account B and read initial progress

Call `get_progress`:

- course_id: **0**
- learner_address: **Account B's full 0x address**

Expected: completed_steps **0**, next_checkpoint **0**, total_steps **2**, course_complete **false**, can_attempt **true**.

### 3. Confirm the second checkpoint is locked

Under `submit_answer`, enter:

- course_id: **0**
- expected_course_hash: **the hash from get_course**
- checkpoint_index: **1**
- answer: the contents of [answer_checkpoint_1.txt](answer_checkpoint_1.txt).

Send once. This is an **intentional negative test**: expect `Submit only your next checkpoint; refresh progress.` No registered attempt should be saved. Read get_counts and get_progress to confirm attempts **0**, completed_steps **0**.

### 4. Submit an incomplete first answer

Keep the course ID and hash, change checkpoint_index to **0**, and use:

```text
I take just one puzzle box. After using it, I put it back on the blue shelf.
```

Expected saved decision: RETRY, C2 MISSING. Call `get_attempt_summary` with attempt_id **0**. Expected progress_before **0**, progress_after **0**, progress_advanced **false**. Check get_progress still has next_checkpoint **0**.

### 5. Pass the first checkpoint

Submit the same course ID/hash and checkpoint_index **0**, using [answer_checkpoint_0.txt](answer_checkpoint_0.txt):

```text
I take just one puzzle box. I log its code so the next volunteer can trace who has it. After using it, I put it back on the blue shelf.
```

Call `get_attempt_summary` with attempt_id **1**. Expected PASSED, all criteria MET, extra NONE, progress_advanced true, progress_before 0, progress_after 1. Call get_progress again: next_checkpoint **1**.

### 6. Confirm a stale replay cannot credit checkpoint 0 twice

Send exactly that first-checkpoint submission again, still with checkpoint_index **0**. Expect the next-checkpoint error. Registered attempts should remain **2**, and completed_steps should remain **1**. This is a second intentional negative test, not a broken contract.

### 7. Pass the second checkpoint

Set checkpoint_index **1** and use [answer_checkpoint_1.txt](answer_checkpoint_1.txt):

```text
I restore the starting arrangement and count the pieces before closing the box. If a piece is missing, I add a missing-piece note inside and do not mark it complete.
```

Expected saved attempt_id **2**, PASSED, progress_before 1, progress_after 2. Get_progress should show completed_steps **2**, course_complete **true**, next_checkpoint **-1**, can_attempt **false**. The -1 means no remaining checkpoint.

### 8. Check wallet separation

Call get_progress for Account A or a third wallet. Its completed_steps should still be **0**. Account A's can_attempt is false because it authored this course. Progress is bound to the submitting wallet, not to everyone viewing the contract.

### 9. Account A closes the course

Switch to Account A. Open `close_course`, enter course_id **0** and the same course hash. Send and confirm success. Get_course should show open **false**. Get_progress for B should retain completed_steps **2** and course_complete **true**, with course_open **false**. The saved attempts and hashes must remain unchanged.

Closure stops future submissions; it does not erase or revoke historical completion. You may optionally confirm a later B submission fails with `Course is closed.`

## D. If something fails

- Expand the transaction and copy the complete error detail and consensus history.
- Confirm the deployed file/version, active wallet, course ID, course hash and next_checkpoint.
- Call get_counts. Do not read an attempt/result ID that has not been created.
- Send the error screenshot and exact input values. Redo only the operation that failed after identifying the cause.
- Local fixture tests do not guarantee live model agreement. A malformed model report, model timeout or validator disagreement needs investigation rather than being counted as a pass.

## E. GitHub upload

After Studio tests, create a public `explaingate` repository. Upload the files **inside** the extracted folder. This package deliberately keeps every file in one folder, including tests and sample inputs. Keep filenames unchanged, especially `explaingate.py`, `demo_course.json` and `test_explaingate.py`. Update the hosted results record with observed results, contract address and evidence links before submission. Do not mark pending tests as completed.
