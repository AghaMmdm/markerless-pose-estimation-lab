# Experiment Results

## Experiment: exp_001 (MoveNet Baseline)

### Objective
Run MoveNet Lightning on sample walking video to establish baseline pose estimation quality.

### Method
- Model: MoveNet Lightning (TensorFlow Hub)
- Input resolution: 192x192
- Confidence threshold: 0.3
- Device: CPU

### Environment
- Python: 3.12.3
- OpenCV: Not yet measured.
- TensorFlow: Not yet measured.
- OS: Ubuntu Linux

### Quantitative Results
| Metric | Value |
|--------|-------|
| Mean keypoint confidence | Not yet measured. |
| Percentage frames with person detected | Not yet measured. |
| Processing FPS | Not yet measured. |
| Mean per-joint error (vs ground truth) | N/A (no ground truth) |

### Qualitative Observations
Not yet measured.

### Limitations
- Single-person model only
- 17 COCO keypoints (no hands/feet detail)
- CPU inference may be slow

### Conclusion
Not yet measured.

### Next Experiment
exp_002: PoseNet comparison on same video.

---

## Experiment: exp_002 (PoseNet Comparison)

### Objective
Compare PoseNet (ResNet50) with MoveNet on same video.

### Method
Not yet measured.

### Environment
Not yet measured.

### Quantitative Results
Not yet measured.

### Qualitative Observations
Not yet measured.

### Limitations
Not yet measured.

### Conclusion
Not yet measured.

### Next Experiment
exp_003: OpenPose evaluation.

---

## Experiment: exp_003 (OpenPose Evaluation)

### Objective
Evaluate OpenPose BODY_25 model on sample video.

### Method
Not yet measured.

### Environment
Not yet measured.

### Quantitative Results
Not yet measured.

### Qualitative Observations
Not yet measured.

### Limitations
Not yet measured.

### Conclusion
Not yet measured.

### Next Experiment
exp_004: DeepLabCut custom training.

---

## Experiment: exp_004 (DeepLabCut Custom Training)

### Objective
Train custom DeepLabCut model for specific keypoints.

### Method
Not yet measured.

### Environment
Not yet measured.

### Quantitative Results
Not yet measured.

### Qualitative Observations
Not yet measured.

### Limitations
Not yet measured.

### Conclusion
Not yet measured.

### Next Experiment
Define research questions and evaluation protocol.

---

*This file will be updated after each experiment is completed.*