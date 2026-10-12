# ExplainGate 0.2: case-3 recovery

**Historical recovery guide.** The version-0.2 deployment subsequently saved case 3 as result 0 with the expected RETRY / C3 INCORRECT / CONTRADICTS_REFERENCE outcome. All six demos and registered learner completion were later observed. See [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md) for that evidence and the remaining blank-reason reliability issue. The deployment instructions below describe the earlier v0.1-to-v0.2 transition; current users do not need another deployment.

## What was observed on version 0.1

On 12 October 2026 the user supplied contract address `0xbCD464dCEeC4dd95d33034B1Fe62dc0773dd443a` and Studio screenshots. No RPC receipt was independently retrieved.

- Case 1: an Accepted-state `get_demo_summary(0)` screenshot showed PASSED, READY, C1/C2/C3 MET, extra NONE and progress_advanced false.
- Case 3: the transaction input showed `run_demo` with case_id 3. Execution displayed FINALIZED / SUCCESS, but consensus history ended UNDETERMINED after leader rotations and multiple Disagree votes. This does not establish a saved demo result. A resulting get_counts read was not supplied.
- The user also pasted the assessment saved here as [case3_v01_observed_report.json](case3_v01_observed_report.json). It is a proposed assessment from the failed run, not an accepted result or a validator's private report.
- Case 2 is partially visible behind the transaction dialog; its complete saved result and finality have not been independently verified here. Cases 4-6 and the registered workflow remain unverified.

## Diagnosis and bounded correction

The supplied report marks the wrong shelf as both C3 INCORRECT and extra CONTRADICTS_REFERENCE. Version 0.1 tells models that extra excludes content already classified under a criterion. A model following that partition can instead return extra NONE.

A local version-0.1 reproduction confirmed that those two reports both yield RETRY but the independent validator rejects their different extra statuses. This is a concrete likely explanation for the observed disagreement. Individual live validator reports are unavailable, so other causes cannot be ruled out.

Version 0.2 removes the partition: the field named extra is explicitly a whole-answer consistency and instruction check, including assertions covered by a criterion. Therefore the wrong shelf is both C3 INCORRECT and extra CONTRADICTS_REFERENCE. Instruction attempts still have highest priority in this check. No numeric score or double penalty is introduced.

The independent assessment, exact comparison of readiness and all classification fields, evidence audit, quotation checks and progress rules remain in place. No disagreement is ignored or converted into a pass. This is a general rubric clarification, not a hardcoded response for case 3.

Four added fixture tests cover the supplied report in preview and registered paths, disagreement rejection and evidence-audit rejection. They do not establish that hosted models will follow the revised prompt or reach consensus.

GenLayer documentation explains that execution and consensus are distinct and undetermined consensus does not commit a state change: https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle . Check stored records as well as transaction badges.

## Deploy and test the failed case first

1. Download the updated `explaingate.py` from this package. Replace the full editor contents, including the first dependency line.
2. Use **Deploy new instance**, Normal (Full Consensus), no constructor inputs. Keep Simulation Mode unchecked. This prototype has no configured state-preserving upgrade workflow; the new version needs a fresh address.
3. Call `get_version`: expect `explaingate-0.2`. Copy the new address.
4. Run **case_id 3 first**, once. Wait for execution success and accepted consensus. If the history ends UNDETERMINED, stop and share the report; do not keep resubmitting.
5. On that fresh instance, call `get_demo_summary` with **result_id 0**. Expected: case_id 3, decision RETRY, C1/C2 MET, C3 INCORRECT, extra CONTRADICTS_REFERENCE, readiness READY, progress_advanced false.
6. After confirming this result, continue using the mapping below. Each mapping assumes exactly one successful saved result per preceding case and no other demos.

| Run order | case_id | result_id | Expected |
| --- | --- | --- | --- |
| 1 | 3 | 0 | RETRY; C3 INCORRECT; extra CONTRADICTS_REFERENCE |
| 2 | 1 | 1 | PASSED; all MET; extra NONE |
| 3 | 2 | 2 | RETRY; C2 MISSING; extra NONE |
| 4 | 4 | 3 | RETRY; extra CONTRADICTS_REFERENCE |
| 5 | 5 | 4 | RETRY; extra INSTRUCTION_ATTEMPT |
| 6 | 6 | 5 | REVIEW_REQUIRED; readiness CONFLICTING; answer NOT_ASSESSED |

This table describes the intended recovery order; the separate hosted record now documents its observed results. After all six previews, the user observed get_counts with demo_results 6, courses 0 and attempts 0, then completed the registered workflow. The default sequential demo table in [DEMO_GUIDE.md](DEMO_GUIDE.md) assumes case order 1-6; do not use its result-ID column for this case-3-first deployment.
