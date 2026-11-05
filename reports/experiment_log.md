# Experiment Log

| Experiment ID | Date | Method | Input | Configuration | Result | Status | Notes |
|---------------|------|--------|-------|---------------|--------|--------|-------|
| exp_001 | 2026-10-23 | MoveNet Lightning (TFLite) | sample_walking.mp4 | lightning, 192×192, cpu, letterbox | 18 valid knee angles (conf ≥ 0.3), 13/697 keypoints out-of-bounds | COMPLETED | First MoveNet baseline with letterbox preprocessing. Sudden angle change at 2.669–3.003s investigated. Right knee confidence dip at frame 90. |
| exp_002 | 2026-11-05 | MoveNet vs DeepLabCut comparison | sagittal_walking.mp4 | lightning, 192×192, cpu, letterbox vs SuperAnimal-Human/RTMPose-X, GPU, zero-shot | Left knee: MAE 20.94°, corr −0.073, 51 valid pairs; Right knee: MAE 9.34°, corr 0.732, 71 valid pairs | COMPLETED | MoveNet vs DLC comparison on sagittal video. DLC detection coverage higher (82/82 right knee vs 71/82). Agreement ≠ accuracy. |
| exp_003 | YYYY-MM-DD | PoseNet | sample_walking.mp4 | resnet50, cpu | Not yet measured. | PLANNED | PoseNet comparison |
| exp_004 | YYYY-MM-DD | OpenPose | sample_walking.mp4 | body_25, cpu | Not yet measured. | PLANNED | OpenPose multi-person |
| exp_005 | YYYY-MM-DD | DeepLabCut | sample_walking.mp4 | custom_keypoints | Not yet measured. | PLANNED | Custom training |

---
*Experiments executed: exp_001 (MoveNet Lightning baseline), exp_002 (MoveNet vs DLC comparison). Updated after each experiment.*