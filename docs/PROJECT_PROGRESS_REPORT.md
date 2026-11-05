# Project Progress Report — Markerless Pose Estimation Lab

**Date:** 2026-11-01  
**Repository:** markerless-pose-estimation-lab  
**Project Path:** ~/Desktop/Programming/markerless-pose-estimation-lab  
**Git Status:** Modified + untracked files present (uncommitted)

---

## 1. Project Objective and Research Direction

**Objective:** An educational, step-by-step project to learn markerless human pose estimation through hands-on experimentation, progressing from OpenCV video processing through MoveNet, PoseNet, OpenPose, and DeepLabCut.

**Research Direction:**
- Stage 1–2: Video processing and MoveNet inference on a frontal-view walking video (COMPLETED)
- Stage 3: Quantitative gait analysis on a sagittal-plane video (COMPLETED)
- Stage 4: DeepLabCut fundamentals and pretrained inference (COMPLETED)
- Stage 5: MoveNet vs DeepLabCut comparison (COMPLETED)
- Stage 6: Research experiments: alternative anatomical landmarks (PLANNED)
- Stage 7: Evaluation and research questions (NOT STARTED)

**Learning Goals:** Video I/O with OpenCV, keypoint extraction, coordinate mapping, joint-angle calculation, temporal visualization, occlusion analysis, and model reliability evaluation.

---

## 2. Current Repository Structure

```
markerless-pose-estimation-lab/
├── README.md
├── LICENSE
├── .gitignore
├── .editorconfig
├── requirements.txt
├── pyproject.toml
├── data/
│   ├── README.md
│   ├── raw/
│   │   ├── .gitkeep
│   │   ├── sample_walking.mp4      (tracked? untracked)
│   │   └── sagittal_walking.mp4    (untracked, 32 MB, NEW)
│   ├── interim/
│   │   ├── .gitkeep
│   │   └── extracted_frames/       (17 JPG frames)
│   └── processed/
│       ├── .gitkeep
│       ├── annotated_walking.mp4   (untracked)
│       ├── annotated_walking_v2.mp4 (untracked)
│       └── resized_walking.mp4     (untracked)
├── notebooks/
│   ├── README.md
│   ├── stage_1_video_processing.ipynb    (committed)
│   ├── stage_2_pose_estimation.ipynb     (modified, uncommitted)
│   └── stage_3_gait_analysis.ipynb       (modified, uncommitted)
├── models/
│   └── movenet_lightning.tflite      (untracked, .gitignore ignores models/)
├── configs/
│   ├── README.md
│   └── .gitkeep
├── experiments/
│   ├── README.md
│   └── experiment_template.yaml
├── reports/
│   ├── README.md
│   ├── progress.md
│   ├── experiment_log.md
│   ├── results.md
│   ├── stage_1_video_processing.md
│   └── stage_2_pose_estimation.md
├── docs/
│   ├── README.md
│   ├── learning_roadmap.md
│   ├── concepts.md
│   ├── tools_comparison.md
│   ├── dataset_card.md
│   ├── setup_guide.md
│   ├── research_notes.md
│   └── decisions/
│       └── README.md
├── scripts/
│   ├── README.md
│   └── .gitkeep
├── src/
│   ├── README.md
│   └── .gitkeep
└── tests/
    ├── README.md
    └── .gitkeep
```

---

## 3. Completed Stages and Evidence

### Stage 1: OpenCV Video Processing — COMPLETED

**Evidence:**
- `notebooks/stage_1_video_processing.ipynb` — executed notebook (37 code cells)
- `reports/stage_1_video_processing.md` — detailed experimental report
- `reports/experiment_log.md` — experiment tracking table
- `data/raw/sample_walking.mp4` — downloaded sample video (8.9 MB)
- `data/interim/extracted_frames/` — 17 extracted frames (14 interval + 3 timestamp)
- `data/processed/resized_walking.mp4` — resized video output (7.0 MB)
- `data/processed/annotated_walking.mp4` — annotated video v1 (18.4 MB)
- `data/processed/annotated_walking_v2.mp4` — annotated video v2 (16.7 MB)

**Key outputs verified:**
- Video: 1080 × 1920 (portrait), 29.97 FPS, 405 frames, ~13.5 seconds
- Frame reading, seeking, timestamp sampling, extraction confirmed
- Resize (direct + aspect-ratio-preserving + letterbox) implemented
- Video writing with cv2.VideoWriter (mp4v codec) confirmed
- Annotation drawing (circle, line, rectangle, text) demonstrated
- Reusable functions `get_video_info()` and `process_video()` implemented

---

### Stage 2: MoveNet Inference & Analysis — COMPLETED

**Evidence:**
- `notebooks/stage_2_pose_estimation.ipynb` — 86 executed cells (modifications uncommitted)
- `models/movenet_lightning.tflite` — TFLite model (4.8 MB, untracked due to .gitignore)
- `reports/stage_2_pose_estimation.md` — detailed experimental report
- `reports/experiment_log.md` — exp_001 marked COMPLETED

**Key outputs verified (from notebook outputs):**
- Model loaded: input 1×192×192×3 uint8, output 1×1×17×3 float32
- Runtime: ai-edge-litert (TFLite), 4 threads
- 41 frames sampled (every 10 frames), 17 keypoints × 41 = 697 predictions
- Skeleton visualization with confidence threshold 0.3
- Confidence analysis (left hip highest mean ~0.49; right knee lowest ~0.37)
- Knee angle calculation: 18 valid estimates out of 41 (confidence ≥ 0.3)
- Knee angle range: 143.27°–179.81°, mean 169.54°
- Sudden angle change investigated at 2.669–3.003 s (−21.20° in 0.33 s)
- Bilateral leg geometry compared at frames 80, 90, 100
- Letterbox preprocessing implemented and validated (scale=0.1, pad_left=42)
- Coordinate inverse transform validated (center point maps correctly)
- 13/697 keypoints (1.9%) mapped outside image bounds after inverse transform

**Letterbox pipeline details:**
- Original: 1080 × 1920 → Resized content: 108 × 192 → Canvas: 192 × 192
- Scale factor: min(192/1080, 192/1920) = 0.1
- Padding: 42 px left/right, 0 px top/bottom
- Inverse transform: x_orig = (x_model − 42) / 0.1, y_orig = y_model / 0.1

---

## 4. Current Stage and Unfinished Tasks

### Current Stage: Stage 3 — Quantitative Gait Analysis — COMPLETED

**Evidence:**
- `notebooks/stage_3_gait_analysis.ipynb` — 22 cells, cells 2–21 executed (cell 20 empty)
- Modified but not committed (shown as `M notebooks/stage_3_gait_analysis.ipynb` in git status)
- Sagittal video acquired: `data/raw/sagittal_walking.mp4` (32 MB, untracked)

**Stage 3 completed work (from notebook outputs):**
- Sagittal video metadata: 1920 × 1080, 30 FPS, 406 frames, ~13.5 seconds
- Video visualized at timestamps 1s, 4s, 7s, 10s
- Frame sampling visualized (8 frames at 1-second intervals)
- MoveNet loaded successfully with letterbox preprocessing function
- 82 frames processed (every 5th frame), 6 lower-limb keypoints extracted per frame
- `calculate_angle()` function defined and tested (straight=180°, right=90°, 45°=45°)
- Left knee angles: 53 valid out of 82; Right knee angles: 71 valid out of 82
- Knee angle visualization plotted (time vs. angle for both legs)

---

### Stage 4: DeepLabCut Fundamentals & Pretrained Inference — COMPLETED

**Evidence:**
- `notebooks/stage_4_deeplabcut_fundamentals.ipynb` — 39 cells, cells 1–38 executed (cell 38 empty markdown)
- Modified but not committed (shown as `M notebooks/stage_4_deeplabcut_fundamentals.ipynb` in git status)

**DeepLabCut environment:**
- DeepLabCut 3.0.2, PyTorch 2.14.1+cu126, CUDA available
- NVIDIA RTX 4060 Laptop GPU
- Python environment: separate `.venv-dlc` from main project venv

**Pretrained inference (Zero-shot):**
- Model: SuperAnimal-Human / RTMPose-X
- Detector: Faster R-CNN MobileNet V3 Large FPN
- Inference type: Top-down (person detection → pose estimation)
- Input video: `data/raw/sagittal_walking.mp4` (406 frames, 1920×1080, 30 FPS)
- Inference time: ~21 seconds (notebook output)
- `video_adapt=False` (no model adaptation), `batch_size=1`, `device="auto"`

**Generated artifacts (`data/processed/deeplabcut/`):**
| File | Size | Description |
|------|------|-------------|
| `sagittal_walking_superanimal_humanbody_rtmpose_x_fasterrcnn_mobilenet_v3_large_fpn.h5` | 960 KB | HDF5 with MultiIndex DataFrame: scorer × individuals × bodyparts × coords |
| `sagittal_walking_superanimal_humanbody_rtmpose_x_fasterrcnn_mobilenet_v3_large_fpn_before_adapt.json` | 1.8 MB | JSON metadata of predictions |
| `sagittal_walking_superanimal_humanbody_rtmpose_x_fasterrcnn_mobilenet_v3_large_fpn_labeled_before_adapt.mp4` | 13.4 MB | Labeled video with overlaid predictions |
| `*_full.pickle` | — | Full pickle results (referenced in notebook output) |

**HDF5 structure:**
- 406 rows (frames), 510 columns (MultiIndex: scorer × 10 individuals × 17 bodyparts × 3 coords)
- Only `animal0` has predictions (406/406 valid frames); animals 1–9 are empty
- Coords: x, y, likelihood per bodypart

**Lower-body keypoint confidence (animal0, 406 frames each):**
| Keypoint | Mean Confidence | Min | Max |
|----------|----------------|-----|-----|
| left_hip | 0.439 | 0.271 | 0.686 |
| right_hip | 0.447 | 0.272 | 0.728 |
| left_knee | 0.664 | 0.419 | 0.965 |
| right_knee | 0.669 | 0.419 | 0.960 |
| left_ankle | 0.622 | 0.278 | 0.898 |
| right_ankle | 0.619 | 0.403 | 0.912 |

**Knee angles calculated:** Left and right knee angle trajectories computed using 3-point geometric method (hip-knee-ankle) and plotted.

---

### Stage 5: MoveNet vs DeepLabCut Comparison — COMPLETED

**Evidence:**
- Comparison implemented in `notebooks/stage_4_deeplabcut_fundamentals.ipynb` (cells 19–35)
- Detailed report: `reports/stage_5_movenet_dlc_comparison.md`
- Comparison outputs in `data/processed/comparison/`:
  - `movenet_dlc_knee_angles.csv` — paired knee angles for 82 sampled frames
  - `agreement_metrics.csv` — MAE, RMSE, Pearson correlation per knee
  - `bias_analysis.csv` — mean bias, difference std, min/max difference per knee
  - `detection_coverage.csv` — valid detection counts per model per knee

**Key results:**
| Metric | Left Knee | Right Knee |
|--------|-----------|------------|
| Valid paired frames | 51 | 71 |
| MAE (degrees) | 20.944 | 9.341 |
| RMSE (degrees) | 26.099 | 12.493 |
| Pearson correlation | −0.073 | 0.732 |
| Mean bias (degrees) | 4.363 | 0.309 |
| Difference std (degrees) | 25.987 | 12.578 |

**Detection coverage:**
| Model | Left Knee | Right Knee |
|-------|-----------|------------|
| MoveNet | 53/82 | 71/82 |
| DeepLabCut | 80/82 | 82/82 |

**Key finding:** Right knee shows strong model agreement (correlation 0.732, MAE 9.34°). Left knee shows weak agreement (correlation −0.073, MAE 20.94°), likely due to occlusion during leg crossing. DeepLabCut provides more consistent detection coverage (82/82 right knee vs MoveNet 71/82). Agreement ≠ accuracy — neither model is ground truth.

---

### Stages 6–7: NOT STARTED

| Stage | Topic | Status |
|-------|-------|--------|
| 6 | Research experiments: alternative anatomical landmarks | PLANNED |
| 7 | Evaluation and research questions | NOT STARTED |

---

## 5. Important Implemented Functions and Their Purposes

| Function | Notebook | Purpose |
|----------|----------|---------|
| `predict_pose(frame)` | stage_2 (cell 47) | MoveNet inference on a single frame (direct resize) |
| `predict_pose_letterboxed(frame, ...)` | stage_2 (cell 88) / stage_3 (cell 214) | MoveNet inference with letterbox preprocessing + inverse coordinate transform |
| `calculate_angle(point_a, point_b, point_c)` | stage_2 (cell 66) / stage_3 (cell 632) | 2D geometric angle from three points (hip-knee-ankle) |
| `get_video_info(video_path)` | stage_1 (cell 21) | Returns dict with fps, frame_count, width, height |
| `process_video(input_path, output_path)` | stage_1 (cell 23) | Frame-by-frame video processing with safe resource cleanup |
| `display_pose_frame(frame_index, keypoints, title)` | stage_2 (cell 42) | Visualize pose landmarks on a video frame |
| `display_both_legs(frame_index, keypoints, title)` | stage_2 (cell 46) | Visualize bilateral lower-limb keypoints |

---

## 6. Dataset/Video Information

### Sample Video (Stage 1 & 2)

| Property | Value |
|----------|-------|
| File | `data/raw/sample_walking.mp4` |
| Resolution | 1080 × 1920 (portrait) |
| FPS | 29.97 |
| Frame count | 405 |
| Duration | ~13.5 s |
| Camera view | Frontal |
| License | Pexels License (redistribution permitted) |

**Limitation:** Frontal view is poorly suited for sagittal-plane flexion/extension measurements.

### Sagittal Walking Video (Stage 3)

| Property | Value |
|----------|-------|
| File | `data/raw/sagittal_walking.mp4` |
| Resolution | 1920 × 1080 (landscape) |
| FPS | 30.0 |
| Frame count | 406 |
| Duration | ~13.5 s |
| Camera view | Sagittal (side view) |
| License | Unknown — not documented in dataset_card.md |

**Key finding:** The sagittal-plane video is now available and is suitable for true gait analysis (hip/knee/ankle flexion/extension visible from side view). However, its license/source is not documented.

---

## 7. Models and Libraries Currently Used

| Component | Version/Source |
|-----------|---------------|
| Python | 3.12.3 |
| OpenCV | 4.x (via opencv-python) |
| NumPy | 1.26+ |
| Pandas | 2.1+ |
| Matplotlib | 3.8+ |
| Jupyter | 1.0+ |
| ai-edge-litert (TFLite runtime) | Installed (not in requirements.txt) |
| DeepLabCut | 3.0.2 (separate `.venv-dlc` environment) |
| PyTorch (for DLC) | 2.14.1+cu126 |
| MoveNet Lightning TFLite | 4.8 MB model |
| MoveNet input | 192 × 192 × 3 uint8 |
| MoveNet output | 1 × 1 × 17 × 3 float32 [y, x, confidence] |
| SuperAnimal-Human / RTMPose-X | DLC pretrained model |
| DLC detector | Faster R-CNN MobileNet V3 Large FPN |

**Missing from requirements.txt:** `ai-edge-litert` (TFLite) is used but not listed. DeepLabCut dependencies are intentionally NOT in the main requirements file — they belong in a separate `.venv-dlc` environment.

---

## 8. Technical Problems and Limitations

### Essential Issues (blocking research progress)

1. **Frontal-view video unsuitable for sagittal-plane gait analysis** — Stage 2 video captures abduction/adduction poorly; hip/knee/ankle flexion primarily occurs in sagittal plane. **Mitigation:** Sagittal video now available (`data/raw/sagittal_walking.mp4`).

2. **No ground truth keypoints** — Cannot compute quantitative accuracy metrics (PCK, OKS, MPJPE). All evaluation is qualitative.

3. **Keypoint occlusion and mislocalization** — Right knee confidence dips to 0.288 during leg overlap (Stage 2, frame 90). Left/right leg swaps observed during occlusion.

4. **Camera movement detected** — Displaces all keypoints simultaneously, confounding displacement analysis.

5. **Letterbox padding outliers** — 13/697 keypoints (1.9%) map outside image bounds after inverse transform (padding region).

6. **ai-edge-litert not in requirements.txt** — Dependency not documented; new environment setup will fail.

7. **Sagittal video license undocumented** — Source and redistribution rights for `sagittal_walking.mp4` not recorded.

8. **DeepLabCut pretrained model limitations** — SuperAnimal-Human provides fixed 17 COCO-style keypoints; does not support custom anatomical landmark definition without manual annotation and training.

### Optional Improvements (not blocking)

1. Temporal smoothing (Kalman filter, EMA) for angle time series
2. Hip and ankle angle calculations (Stage 3 only does knee)
3. Confidence bands on angle plots
4. Saved CSV exports for keypoints and angles
5. Experiment config YAML files (currently only template exists)

---

## 9. Recommended Next Steps

### Immediate (Documentation & Repository Sync)
1. Commit the uncommitted notebook changes (`stage_2_pose_estimation.ipynb`, `stage_3_gait_analysis.ipynb`, `stage_4_deeplabcut_fundamentals.ipynb`)
2. Add `ai-edge-litert` to `requirements.txt`
3. Document sagittal video source/license in `docs/dataset_card.md`
4. Write `reports/stage_3_gait_analysis.md`
5. Update `reports/experiment_log.md` with exp_002 (MoveNet vs DLC comparison on sagittal video)

### Short-term (Stage 4 & Stage 5: DeepLabCut comparison)
### Short-term (Stage 6: Research Experiments — Alternative Anatomical Landmarks)
1. Investigate whether adding anatomical landmarks beyond hip–knee–ankle can improve knee-angle estimation
   - Additional landmarks along thigh and shank
   - Additional landmarks around the knee joint
   - Segment orientation estimates using multiple landmarks
   - Compare alternative configurations against the conventional three-point baseline
2. **Constraint:** Exact landmark configuration not yet finalized with supervisor — do not start manual annotation or training yet
3. **Constraint:** Pretrained SuperAnimal-Human provides fixed COCO-style keypoints; custom landmarks require annotated dataset and model adaptation/training
4. **Constraint:** Independent ground-truth measurements are necessary to evaluate actual accuracy
5. Define research questions (e.g., "How does camera angle affect knee angle accuracy?")
6. Design evaluation protocol without ground truth

### Medium-term (Stage 7: Evaluation and Research Questions)
1. Compare MoveNet vs DeepLabCut on sagittal video (already completed in Stage 5)
2. Quantify occlusion handling improvement with custom landmarks
3. Document research findings and limitations
4. Formulate final research conclusions

---

## 10. Proposed Roadmap: MoveNet → DeepLabCut Transition

### Phase A: Complete Stage 3 (Current — Documentation Only)
- Write `reports/stage_3_gait_analysis.md`
- Commit notebook changes

### Phase B: Stage 4 — Reliability and Limitations (Completed)
- DeepLabCut pretrained inference on sagittal video ✅
- MoveNet vs DLC comparison on sagittal video ✅

### Phase C: Stage 6 — Research Experiments: Alternative Anatomical Landmarks
1. **Constraint:** Exact landmark configuration not yet finalized with supervisor — do not start manual annotation or training yet
2. **Constraint:** Pretrained SuperAnimal-Human provides fixed COCO-style keypoints; custom landmarks require annotated dataset and model adaptation/training
3. **Constraint:** Independent ground-truth measurements are necessary to evaluate actual accuracy
4. Investigate alternative landmark configurations (thigh/shank landmarks, additional knee landmarks)
5. Compare alternative configurations against conventional three-point baseline

### Phase D: Stage 7 — Evaluation and Research Questions
1. Define research questions (e.g., "How does camera angle affect knee angle accuracy?")
2. Design evaluation protocol without ground truth
3. Document limitations and uncertainty quantification
4. Formulate final research conclusions

---

## Summary

| Stage | Status | Evidence |
|-------|--------|----------|
| Stage 1: Video Processing | COMPLETED | Notebook executed, report written |
| Stage 2: MoveNet Inference | COMPLETED | Notebook executed (86 cells), report written, experiment logged |
| Stage 3: Gait Analysis | COMPLETED | Notebook executed (22 cells), sagittal video processed, knee angles calculated |
| Stage 4: DeepLabCut Fundamentals | COMPLETED | Notebook executed (39 cells), pretrained inference on sagittal video, 406 frames processed, lower-body keypoints extracted, knee angles calculated |
| Stage 5: MoveNet vs DLC Comparison | COMPLETED | Notebook comparison cells (19–35), comparison report written, 4 CSV outputs generated |
| Stage 6: Research Experiments | NOT STARTED | — |
| Stage 7: Evaluation and Research Questions | NOT STARTED | — |

**Key assets available:** MoveNet TFLite model, frontal video, sagittal video, letterbox preprocessing pipeline, coordinate inverse transform, angle calculation function, bilateral keypoint comparison framework.

**Critical gap:** No sagittal-plane video license documentation; Stage 3 and Stage 4 notebook modifications uncommitted; `ai-edge-litert` missing from requirements.txt.

---

## Files Inspected for This Audit

1. `README.md` — project overview, roadmap, status
2. `requirements.txt` — dependency list
3. `.gitignore` — ignore patterns
4. `reports/progress.md` — project progress tracker
5. `reports/experiment_log.md` — experiment tracking table
6. `reports/results.md` — experiment results template
7. `reports/stage_1_video_processing.md` — Stage 1 report
8. `reports/stage_2_pose_estimation.md` — Stage 2 detailed report
9. `reports/README.md` — reports index
10. `notebooks/stage_1_video_processing.ipynb` — Stage 1 notebook (committed)
11. `notebooks/stage_2_pose_estimation.ipynb` — Stage 2 notebook (modified, uncommitted)
12. `notebooks/stage_3_gait_analysis.ipynb` — Stage 3 notebook (modified, uncommitted)
13. `docs/learning_roadmap.md` — learning roadmap
14. `docs/dataset_card.md` — dataset/video provenance
15. `docs/setup_guide.md` — environment setup instructions
16. `docs/research_notes.md` — research notes template
17. `docs/concepts.md` — glossary
18. `docs/tools_comparison.md` — tool comparison
19. `models/movenet_lightning.tflite` — MoveNet model (4.8 MB)
20. `data/raw/sample_walking.mp4` — frontal walking video (8.9 MB)
21. `data/raw/sagittal_walking.mp4` — sagittal walking video (32 MB, new)
22. `data/interim/extracted_frames/` — 17 extracted frames
23. `data/processed/*.mp4` — 3 processed videos (untracked)
24. `experiments/experiment_template.yaml` — experiment config template
25. `configs/README.md` — configs documentation
26. `pyproject.toml` — project packaging config
27. `notebooks/stage_4_deeplabcut_fundamentals.ipynb` — Stage 4 notebook (modified, uncommitted)
28. `data/raw/sagittal_walking.mp4` — sagittal walking video (32 MB, untracked)
29. `data/processed/deeplabcut/` — DeepLabCut output directory (untracked)
   - `*.h5` — HDF5 predictions (960 KB, 406 frames × 510 columns)
   - `*_before_adapt.json` — JSON metadata (1.8 MB)
   - `*_labeled_before_adapt.mp4` — labeled video (13.4 MB)