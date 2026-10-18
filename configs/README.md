# Configurations Directory

YAML configuration files for experiments and model parameters.

## Structure

```
configs/
├── movenet.yaml      # MoveNet inference parameters
├── posenet.yaml      # PoseNet parameters
├── openpose.yaml     # OpenPose parameters
├── deeplabcut.yaml   # DeepLabCut training config
├── video.yaml        # Video processing defaults
└── analysis.yaml     # Analysis parameters (angles, smoothing, etc.)
```

## Format

All configs use YAML with the following top-level keys:

```yaml
experiment:
  id: "exp_001"
  name: "MoveNet baseline"
  
model:
  name: "movenet_lightning"
  version: "4"
  
video:
  input: "data/raw/sample_walking.mp4"
  start_frame: 0
  end_frame: null
  skip_frames: 1
  
inference:
  confidence_threshold: 0.3
  input_size: 192
  
output:
  keypoints_csv: "data/processed/keypoints_exp001.csv"
  annotated_video: "data/processed/annotated_exp001.mp4"
```

## Usage

Load in Python:
```python
import yaml
with open("configs/movenet.yaml") as f:
    config = yaml.safe_load(f)
```