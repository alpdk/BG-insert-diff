---

## id: baseline
title: "Baseline"
target: mdlm-puma-flexmdm-editflow
data: tinygsm
hardware: zih-gpu
status: draft

# Baseline

Comparison contract for **MDLM, PUMA, FlexMDM, and EditFlow** on TinyGSM at packed length 256. Every method copies `[configs/shared.yaml](../../../configs/shared.yaml)`. Experiments only record deviations. Knobs: [configs-short.md](../configs-short.md).

## Data

- Train: TinyGSM, packed `question + "\n" + answer`, length 256, cap 100000 examples.
- Val: 2% held-out TinyGSM (`val_ratio: 0.02`, seed 42). Same packing. No sampling.
- Test (in-domain): TinyGSM generate+score, Python exec vs held-out gold code (`test/pass@1_k*`).
- Transfer: GSM8K at the same four `k` (`test/gsm8k/pass@1_k*`). PUMA has no GSM8K train YAML.



## Training

- 260000 steps from scratch. Batch 64, bf16, 768 / 12 / 12, dropout 0.1.
- AdamW `lr` 3e-4 after 500-step warmup, then constant. EMA 0.9999 (EditFlow has none).
- Configs: `[mdlm](../../../configs/mdlm/tinygsm.yaml)`, `[flexmdm](../../../configs/flexmdm/tinygsm.yaml)`, `[puma](../../../configs/puma/tinygsm.yaml)`, `[editflow](../../../configs/editflow/tinygsm.yaml)`.



## Evaluation

- Rank by `val/loss` and `test/pass@1_k{1,2,4,8}`. Not `train/loss`. Keys: [metrics-short.md](../metrics-short.md).
- 32 examples; `k` = 1, 2, 4, 8 tokens per sampling step; 128 sampling steps (PUMA uses stage `k` instead).
- Val and generate+score every 2000 steps. `best` = min `val/loss`. `best-task` = max `"test/pass@1_k1"`.
- W&B: project `edit-diffusion`, group `shared-params`.



## Pipeline

1. Launch each method from its directory with the TinyGSM shared-params config. Order: **MDLM → FlexMDM → PUMA → EditFlow**.
2. Each method trains 260k steps from scratch. Val + generate+score run inside training every 2000 steps (not a separate benchmark).
3. If one method fails, continue with the rest.
4. Logs go to W&B group `shared-params`. Compare methods at the same `k` on `val/loss`, in-domain `test/pass@1_k*`, and GSM8K transfer.

