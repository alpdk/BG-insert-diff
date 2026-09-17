# Shared-params configs (short)

Same knobs for MDLM, PUMA, FlexMDM, EditFlow. Do not change a shared value in one method only.

Full notes and trainer names: [configs.md](configs.md). W&B keys: [metrics-short.md](metrics-short.md).

## Shared knobs

From `configs/shared.yaml`.

| Knob                      | Value              | What |
| ------------------------- | ------------------ | ---- |
| `length`                  | `256`              | Sequence length. EditFlow: 128 prompt + 128 answer. |
| `global_batch_size`       | `64`               | Examples per optimizer step. |
| `eval_global_batch_size`  | `32`               | Smaller batch for val/test (memory). |
| `num_workers`             | `4`                | Dataloader workers. |
| `seed`                    | `42`               | Makes training, the val split, and generation repeatable. |
| `precision`               | `bf16`             | Mixed precision. |
| `grad_clip`               | `1.0`              | Clip gradient L2 to this max. |
| `lr`                      | `3.0e-4`           | Peak AdamW learning rate. |
| `weight_decay`            | `0.03`             | AdamW weight decay. |
| `warmup_steps`            | `500`              | Linear warmup to `lr`, then hold. |
| `lr_schedule`             | `constant`         | After warmup, LR stays at peak. Not cosine. |
| `ema`                     | `0.9999`           | EMA weights at val/test. EditFlow has none. |
| `hidden_size`             | `768`              | Model width. |
| `num_layers`              | `12`               | Transformer depth. |
| `num_heads`               | `12`               | Attention heads (`768 / 12 = 64`). |
| `dropout`                 | `0.1`              | Dropout rate. |
| `val_every`               | `2000`             | Steps between val and generate+score. |
| `ckpt_every`              | `2000`             | Steps between checkpoints. |
| `log_every`               | `100`              | Steps between `train/` logs. |
| `early_stopping_patience` | `9999`             | Off: run the full `max_steps` budget. |
| `resume_from_ckpt`        | `false`            | Do not load a checkpoint. Train from scratch. |
| `task_num_samples`        | `32`               | How many examples to score for `test/pass@1_k*`. |
| `sampling_steps`          | `128`              | Denoising / edit steps at test. PUMA uses stage `k` instead. |
| `pass_at_1_k`             | `[1, 2, 4, 8]`     | Tokens written per sampling step. |
| `generate_samples`        | `true`             | Run task eval at each val interval. |
| `task_checkpoint_monitor` | `"test/pass@1_k1"` | `best-task` checkpoint. Quote the key in YAML. |

Likelihood `best` still tracks min `val/loss`. W&B: project `edit-diffusion`, group `shared-params`.

## Dataset

| Knob            | TinyGSM   | GSM8K         | What |
| --------------- | --------- | ------------- | ---- |
| `max_steps`     | `260000`  | `20000`       | Optimizer steps. Same budget for every method. |
| `max_examples`  | `100000`  | full set      | TinyGSM train cap. |
| `val_ratio`     | `0.02`    | dataset val   | TinyGSM held-out fraction for `val/`. |
| `eval_task`     | `tinygsm` | `gsm8k`       | In-domain `test/pass@1_k*`. |
| `transfer_task` | `gsm8k`   | —             | TinyGSM runs also log `test/gsm8k/pass@1_k*`. |

## Allowed to differ (algorithm / trainer)

Do not “fix” these to match.

| Item | Why it stays |
| ---- | ------------ |
| MDLM `log-linear` vs FlexMDM `linear` noise | Algorithm. |
| PUMA progressive `k_schedule` | Algorithm. Not `sampling_steps`. |
| FlexMDM unmask + length losses | Algorithm. Rank with `val/loss`. |
| EditFlow insert/delete/substitute, `coupling`, `t_eps` | Algorithm. |
| Tokenizer: GPT-2 (UNI-D2) vs Qwen (PUMA) vs bytes (EditFlow) | Sequence *count* is still 256. |
| EditFlow has no EMA | Trainer has no EMA. |
| PUMA `intermediate_size: 2048` | SwiGLU width; no UNI-D2 match. |
| PUMA trains TinyGSM only | GSM8K is transfer eval, not a train YAML. |
| PUMA scores the full val / GSM8K test loop | Not `task_num_samples: 32`. |
