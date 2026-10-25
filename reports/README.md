# Reports Index

This directory contains project progress tracking, experimental logs, and detailed reports.

## Files

| File | Description | Last Updated |
|------|-------------|--------------|
| `progress.md` | Overall project status, completed/pending tasks, known issues | 2026-10-23 |
| `experiment_log.md` | Table of all experiments with status and key results | 2026-10-23 |
| `stage_1_video_processing.md` | Detailed report on Stage 1 video processing exercises | 2026-10-20 |
| `stage_2_pose_estimation.md` | Detailed experimental report for Stage 2 (MoveNet) | 2026-10-23 |
| `results.md` | Template for detailed per-experiment results | 2026-10-18 |
| `figures/` | Generated plots and visualizations | — |

## Experiment Summary

| ID | Method | Stage | Status | Key Result |
|----|--------|-------|--------|------------|
| exp_001 | MoveNet Lightning (TFLite) | 2 | COMPLETED | 18 valid knee angles; letterbox preprocessing; sudden angle change at 3s investigated |
| exp_002 | MoveNet (refined) | 3 | PLANNED | Knee-angle smoothing, hip/ankle angles, occlusion handling |
| exp_003 | PoseNet | 4 | PLANNED | Comparison with MoveNet |
| exp_004 | OpenPose | 5 | PLANNED | Multi-person evaluation |
| exp_005 | DeepLabCut | 6 | PLANNED | Custom keypoint training |

## Quick Links

- [Project Progress](progress.md)
- [Experiment Log](experiment_log.md)
- [Stage 1 Report](stage_1_video_processing.md)
- [Stage 2 Report](stage_2_pose_estimation.md)

---

*Reports are updated after each experimental stage. See `experiment_log.md` for the authoritative experiment table.*