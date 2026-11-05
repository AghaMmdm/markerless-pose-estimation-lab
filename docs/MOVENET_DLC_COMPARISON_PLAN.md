# MoveNet vs DeepLabCut Comparison Plan

**Date:** 2026-11-05  
**Project:** markerless-pose-estimation-lab  
**Status:** Planned (not yet executed)

---

## A. Experimental Objective

Compare the consistency of MoveNet and DeepLabCut pose-estimation predictions on the same sagittal-plane walking video (`data/raw/sagittal_walking.mp4`). The goal is to evaluate whether the two models produce consistent lower-limb keypoint trajectories and knee-angle estimates, and to understand where they agree or diverge.

**This comparison does not establish ground-truth accuracy** — neither model's predictions are independently validated. Agreement between models is not the same as correctness.

---

## B. Frame Alignment

MoveNet and DeepLabCut processed the same video at different sampling rates:

| Model | Video | Frames Processed | Sampling | Frame Indices |
|-------|-------|-----------------|----------|---------------|
| MoveNet Lightning | `data/raw/sagittal_walking.mp4` | 82 | Every 5th frame | 0, 5, 10, ..., 405 |
| DeepLabCut (SuperAnimal-Human) | `data/raw/sagittal_walking.mp4` | 406 | Every frame | 0, 1, 2, ..., 405 |

**Alignment strategy:** The comparison must use matching frame indices. Only MoveNet frames that correspond to DLC frame indices are compared. Since MoveNet samples every 5th frame (0, 5, 10, ..., 405), these same indices exist in the DLC predictions. All other DLC frames (non-multiples of 5) are excluded from the comparison.

**Do not compare different frames** — comparing MoveNet frame 0 against DLC frame 1 would be invalid.

---

## C. Keypoint Comparison

### Keypoints of Interest

| Keypoint | MoveNet Index | DLC Bodypart Name |
|----------|--------------|-------------------|
| Left hip | 11 | `left_hip` |
| Right hip | 12 | `right_hip` |
| Left knee | 13 | `left_knee` |
| Right knee | 14 | `right_knee` |
| Left ankle | 15 | `left_ankle` |
| Right ankle | 16 | `right_ankle` |

### Coordinate Systems

- **MoveNet:** Normalized [y, x] coordinates in [0, 1] relative to original image dimensions (after letterbox inverse transform). Pixel coordinates: x_px = x_norm × 1920, y_px = y_norm × 1080.
- **DeepLabCut:** Pixel coordinates [x, y] directly (already in original image pixel space, 1920 × 1080).

**Normalization required** before comparison: convert both to pixel coordinates in the same 1920 × 1080 frame space.

### Anatomical Definition Check

- MoveNet uses COCO 17-keypoint format (left/right hip, knee, ankle).
- DLC SuperAnimal-Human uses COCO-style 17 body parts (left_hip, right_hip, left_knee, right_knee, left_ankle, right_ankle).
- Both models claim to predict the same anatomical landmarks, but left/right conventions should be verified visually (especially during leg crossing/occlusion).

---

## D. Confidence and Detection Reliability

### Metrics to Compare

| Metric | MoveNet | DeepLabCut |
|--------|---------|------------|
| Valid prediction rate | % frames with confidence ≥ 0.3 | % frames with likelihood ≥ 0 (DLC uses -1.0 as invalid placeholder) |
| Confidence distribution | Per-keypoint mean/min/max | Per-keypoint mean/min/max |
| Missing predictions | Keypoints below threshold | Likelihood = -1.0 (placeholder) |
| Temporal consistency | Frame-to-frame displacement | Frame-to-frame displacement |

**Important:** Confidence scores from MoveNet and DeepLabCut are **not directly calibrated** and should not be treated as comparable probability values. A MoveNet confidence of 0.5 does not mean the same thing as a DLC likelihood of 0.5. Compare confidence *within* each model, not *across* models.

---

## E. Knee-Angle Comparison

### Method

Use the same 3-point geometric method for both models:

```
angle = arccos(dot(v1, v2) / (|v1| × |v2|))
```

where v1 = hip − knee and v2 = ankle − knee, computed in pixel coordinates.

Compute left and right knee angles for each matching frame index.

### Comparison Metrics

| Metric | Description |
|--------|-------------|
| Mean absolute difference | Mean(|angle_MoveNet − angle_DLC|) across valid frames |
| Root mean squared difference | sqrt(mean((angle_MoveNet − angle_DLC)²)) |
| Valid frame percentage | % of matching frames where both models produce valid angles |
| Temporal continuity | Frame-to-frame angle change in each model |

### Interpretation Guidelines

- **Model agreement ≠ measurement accuracy:** If both models produce similar angles, it means they agree with each other, not that either is anatomically correct.
- **Disagreement is informative:** Large differences may indicate occlusion, landmark swap, or model-specific failure modes.
- **Confidence weighting:** Consider weighting comparison by per-keypoint confidence (within each model separately).

---

## F. Limitations

1. **Neither model is ground truth:** MoveNet and DeepLabCut are both predictive models. Their agreement or disagreement cannot establish anatomical accuracy without an independent reference (e.g., motion capture, carefully established reference annotations).

2. **Pretrained DLC model limitations:** The SuperAnimal-Human pretrained model provides fixed COCO-style keypoints. It does not automatically predict custom anatomical landmarks. Custom landmark definition would require manual annotation and model training (a future Stage 6 activity).

3. **Different preprocessing:** MoveNet uses letterbox preprocessing with aspect-ratio preservation and coordinate inverse transform. DLC SuperAnimal-Human uses its own internal preprocessing (person detection → pose estimation). These differences can affect keypoint localization independently of model quality.

4. **Single video, single subject:** Results are specific to this sagittal walking video and may not generalize to other videos, subjects, or camera setups.

5. **2D only:** Both models produce 2D image-plane coordinates. Angles are 2D projections and may not reflect true 3D joint angles.

---

## G. Success Criteria (Informational, Not Pass/Fail)

- Both models produce valid knee-angle estimates for ≥50% of matching frames.
- Mean absolute knee-angle difference is documented (no target threshold).
- Confidence distributions are documented for all six lower-limb keypoints.
- Occlusion events (low confidence, large displacement, anatomical inconsistency) are identified and discussed.

---

## H. Deliverables

1. `reports/movenet_dlc_comparison.md` — detailed comparison report
2. Aligned keypoint data (CSV or similar) for matching frame indices
3. Knee-angle trajectory plot comparing both models
4. Confidence distribution plot for both models
5. Summary of agreement/disagreement findings and limitations

---

## I. Next Step After Comparison

Based on comparison results, decide whether to:
- Proceed with custom DeepLabCut training (Stage 6) using manually annotated frames, or
- Continue with MoveNet-only analysis, or
- Use both models complementarily (e.g., DLC for initialization, MoveNet for refinement)