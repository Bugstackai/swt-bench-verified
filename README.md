# Bugstack on SWT-Bench Verified

Bugstack's predictions, evaluation report and inference traces for [SWT-Bench](https://swtbench.com) Verified (433 instances). The task is to write a test that reproduces a reported issue, inside the repository's own test suite.

| Metric | Result |
|---|---|
| Success rate (S) | **92.61%** (401 / 433) |
| Coverage delta (Δᵃˡˡ) | **75.16%** |
| Applicability (W) | 98.85% |

A second scoring of the same predictions gave 92.38% (400 / 433) and 67.88%. The difference is test flakiness and coverage measurement.

Model: Claude Sonnet 5.5 (Claude Opus 5.5 selects among candidate tests). Project: [bugstack.ai](https://bugstack.ai).

## Contents

| File | What it is |
|---|---|
| `predictions.jsonl` | One prediction per instance: `instance_id`, `model_name_or_path`, `model_patch` (the test, as a git diff against the base commit) |
| `evaluation_report.json` | The SWT-Bench harness's report for these predictions (the scoring above) |
| `evaluation_report_first-scoring.json` | The harness's report from the second scoring of the same predictions |
| `traces.zip` (release asset) | One file per instance with every model call in order: the inputs, tool calls and their results, and every model output |

## Scoring the predictions

With the [SWT-Bench harness](https://github.com/logic-star-ai/swt-bench):

```
python -m src.main --dataset_name princeton-nlp/SWE-bench_Verified --predictions_path predictions.jsonl --filter_swt --run_id bugstack
python -m src.report run_instance_swt_logs/bugstack/bugstack --dataset verified
```

The numbers above come from harness commit `330a649`.

## About the traces

Each trace is a JSON Lines file. A `call` line is one model request and its response; an `event` line marks the start of an instance and the test that was kept.

A small number of static prompt templates are replaced by their SHA-256 hash, each marked with how many instances and repositories share it. Only text identical across at least 20 instances in at least 3 repositories was replaced. `manifest.json` lists every replaced piece.

Never replaced: the issue text, the repository content the method read, tool calls and their results, and model outputs.

## Contact

Mason Bachmann, Founder and CEO, Bugstack · https://bugstack.ai
