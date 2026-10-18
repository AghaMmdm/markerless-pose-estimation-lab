# Experiments Directory

Experiment configuration files and tracking.

## Files

- `experiment_template.yaml` - Template for new experiments
- `exp_<id>_<method>.yaml` - Individual experiment configurations

## Experiment Template

See `experiment_template.yaml` for the standard structure.

## Creating a New Experiment

```bash
cp experiments/experiment_template.yaml experiments/exp_001_movenet.yaml
# Edit the new file with your parameters
```

## Experiment ID Format

`exp_<NNN>_<method>` where:
- NNN = zero-padded sequence number (001, 002, ...)
- method = short method name (movenet, posenet, openpose, deeplabcut)