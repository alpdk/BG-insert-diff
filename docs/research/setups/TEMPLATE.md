---
id: TODO
title: "TODO: short setup name"
target: TODO
data: TODO
hardware: TODO
status: draft
---

# TODO: short setup name

One paragraph on what this setup fixes: the target model, the training data
and the evaluation protocol that every experiment referencing this setup
shares. Experiments describe only their deviation from what is written here.

## Training

- Base config: TODO (link to the YAML under `examples/configs/` and the draft
  JSON under `configs/`).
- Feature source: TODO (online capture or offline features, and how they were produced).

## Environment

- GDFlash commit: TODO
- SGLang version and patch: TODO
- Container or venv: TODO

## Evaluation protocol

- Metrics: acceptance length, output tokens/s, task accuracy.
- Eval datasets: TODO
- Number of prompts per dataset: TODO
- Temperature: TODO
- Batch size: TODO
- Speculation config (num-steps / topk / num-draft-tokens): TODO
- Benchmark command: TODO (link to the script or `specforge benchmark` invocation).

## Results

See the setup's result table, `dashboards/results-<setup-id>.md`, created from
[`dashboards/results-template.md`](../dashboards/results-template.md).
