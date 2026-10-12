# Start ExplainGate in GenLayer Studio

You only need **explaingate.py** for deployment. The ZIP contains the complete project for GitHub and local testing.

This package is version **0.2**. The existing deployment has completed its six-demo and two-checkpoint walkthrough. Its code is unchanged by this documentation update. See [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md) for observed results, retries and deferred checks.

## Resume the existing deployment

Contract address: `0x9897659E227013462b251C507580D1a1BA95687A`.

In Studio, use **Import Contract** beside **Your Contracts** and enter that address. This imports an existing deployed instance. **Add From File** loads source only; deploying a new instance creates separate storage. Open Run and Debug after importing.

The recorded learner `0x507De2c3391F085e99208C28ad4EEAf567995B43` completed both checkpoints of course `0`. No additional submissions are needed for that learner. Read `get_progress` to inspect the current state.

For GitHub, extract the ZIP and upload the files inside its `ExplainGate` folder to the repository root. The remaining deployment steps are for someone intentionally reproducing the project on a new instance.

## First step: deploy

1. Download `explaingate.py`, or extract the starter ZIP and find that file.
2. Open https://studio.genlayer.com/ in Chrome.
3. Create/import a **new** file called `explaingate.py`. Paste/import its complete content, including the first dependency line.
4. Open **Run and Debug**.
5. Select **Normal (Full Consensus)**.
6. Deploy a **new instance**. The constructor takes no arguments.
7. Wait for deployment to complete. Confirm execution and consensus succeeded; FINALIZED or Result SUCCESS alone does not prove an accepted state change. Undetermined consensus is not a successful test.
8. Copy the new contract address and take a screenshot showing deployment.

Keep your earlier NoticeParity instance separate. ExplainGate is a new contract with its own storage.

## After deployment: first demo

1. Open read method `get_version`, then tap **Call Contract**. Expected: `explaingate-0.2`.
2. Under write methods, leave **Simulation Mode unchecked**.
3. Open `run_demo`. Enter **case_id: 1** and tap **Send Transaction** once.
4. Wait for the transaction to finish and confirm execution success and accepted consensus. If the consensus history ends UNDETERMINED, stop and inspect it even when execution says SUCCESS.
5. Open `get_demo_summary`. On a fresh instance, enter **result_id: 0**, then tap **Call Contract**.

Expected: `PASSED`, all three criteria `MET`, extra `NONE`, readiness `READY`, and `progress_advanced: false`.

The false value is correct: demo previews never unlock registered learning steps.

[DEMO_GUIDE.md](DEMO_GUIDE.md) contains the reproduction sequence. Result IDs follow successful save order. The observed deployment used case order 3, 1, 2, 4, 5, 6; its exact mapping is in [DEMO_RESULTS_TEMPLATE.md](DEMO_RESULTS_TEMPLATE.md).
