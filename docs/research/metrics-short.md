# W&B metrics (short)

Rank by `val/loss` and `test/pass@1_k{1,2,4,8}`. Not by `train/loss`. `k` = tokens per sampling step.

Full notes: [metrics.md](metrics.md).

| Prefix     | What                                                      |
| ---------- | --------------------------------------------------------- |
| `train/`   | Loss on the current batch, plus LR. Do not rank by this.  |
| `val/`     | Same loss as training, on val data. No sampling.          |
| `test/`    | Sample an answer, then score it (pass@1).                 |
| `perf/`    | How long it takes. Not a quality score.                   |
| `healthy/` | Is the run numerically OK (gradient size).                |
| `stats/`   | Method internals (PUMA `k`, EditFlow edit rates).         |

## All four models

| Key                       | What                                           |
| ------------------------- | ---------------------------------------------- |
| `train/loss`              | Training loss on the current batch             |
| `train/lr`                | Learning rate after the scheduler              |
| `val/loss`                | Same loss as `train/loss`, on val data         |
| `test/pass@1_k1`          | pass@1, **1** token per sampling step          |
| `test/pass@1_k2`          | pass@1, **2** tokens per sampling step         |
| `test/pass@1_k4`          | pass@1, **4** tokens per sampling step         |
| `test/pass@1_k8`          | pass@1, **8** tokens per sampling step         |
| `perf/train_step_s`       | Seconds for one optimizer step                 |
| `perf/train_tokens_per_s` | Train tokens processed per second              |
| `perf/val_step_s`         | Seconds for one val-loss batch (no sampling)   |
| `perf/test_s`             | Seconds for one generate-then-score pass       |
| `healthy/grad_norm`       | Gradient L2. Before clip (MDLM, EditFlow); after (PUMA); FlexMDM logs both |

## Method extras

| Key                             | Who          | What                                      |
| ------------------------------- | ------------ | ----------------------------------------- |
| `train/unmask_loss`, `len_loss` | FlexMDM      | Unmask + length (`train/loss` = sum)      |
| `val/unmask_loss`, `len_loss`   | FlexMDM      | Same terms on val                         |
| `train/ins_loss`, `del_loss`, `sub_loss` | EditFlow | Insert / delete / substitute (`train/loss` = sum) |
| `stats/u_ins`, `u_del`, `u_sub`, `u_tot`, `u_con` | EditFlow | Mean insert / delete / substitute / total / continue edit rate |
| `stats/current_k`               | PUMA         | Tokens unmasked per progressive stage     |
| `test/gsm8k/pass@1_k1` … `k8`   | TinyGSM runs | GSM8K transfer at the same four `k`       |
