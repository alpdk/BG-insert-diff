---
id: YYYY-MM-DD-method-target-setup-tag
title: "TODO: one-line experiment title"
setup: TODO
method: TODO
status: planned
wandb: TODO
config: TODO
date: YYYY-MM-DD
---

# TODO: one-line experiment title

## Goal

One or two sentences: the question this run answers.

## Difference from baseline

What changes relative to the setup's base config and why. Link the config
file; do not copy its parameters here.

## Reproduction

```bash
# training
specforge train -c examples/configs/<path-to-config>.yaml

# evaluation (see the setup page for the protocol)
specforge benchmark ...
```

## Artifacts

- wandb: TODO (run name equals the experiment id, project `gdflash`)
- Logs: TODO
- Checkpoints: `outputs/<experiment-id>/`

## Conclusion

At most three sentences. Numbers go to the setup's result table in
`dashboards/`, not here.
