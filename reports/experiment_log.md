# Experiment Log

| Experiment ID | Date | Method | Input | Configuration | Result | Status | Notes |
|---------------|------|--------|-------|---------------|--------|--------|-------|
| exp_001 | 2026-10-23 | MoveNet Lightning (TFLite) | sample_walking.mp4 | lightning, 192×192, cpu, letterbox | 18 valid knee angles (conf ≥ 0.3), 13/697 keypoints out-of-bounds | COMPLETED | First MoveNet baseline with letterbox preprocessing. Sudden angle change at 2.669–3.003s investigated. Right knee confidence dip at frame 90. |
| exp_002 | YYYY-MM-DD | MoveNet (refined) | sample_walking.mp4 | lightning, 192×192, cpu, letterbox + smoothing | Not yet measured. | PLANNED | Knee-angle smoothing, hip/ankle angles, occlusion handling |
| exp_003 | YYYY-MM-DD | PoseNet | sample_walking.mp4 | resnet50, cpu | Not yet measured. | PLANNED | PoseNet comparison |
| exp_004 | YYYY-MM-DD | OpenPose | sample_walking.mp4 | body_25, cpu | Not yet measured. | PLANNED | OpenPose multi-person |
| exp_005 | YYYY-MM-DD | DeepLabCut | sample_walking.mp4 | custom_keypoints | Not yet measured. | PLANNED | Custom training |

---
*Experiments executed: exp_001 (MoveNet Lightning baseline with letterbox preprocessing). Updated after each experiment.*