# Stage 2: MoveNet Inference & Analysis — Experimental Report

**Date:** 2026-10-23  
**Experiment ID:** exp_001  
**Status:** COMPLETED  
**Notebook:** `notebooks/stage_2_pose_estimation.ipynb` (94 executed cells)

---

## 1. Objective

Establish a baseline MoveNet Lightning inference pipeline on the sample walking video, extract 17 body keypoints with confidence scores, calculate knee angles across time, investigate sudden angle changes, and implement letterbox preprocessing to preserve aspect ratio.

---

## 2. Environment & Model

| Property | Value |
|----------|-------|
| **Model** | MoveNet Lightning (TFLite) |
| **Model Path** | `models/movenet_lightning.tflite` (4.8 MB) |
| **Input Shape** | 1 × 192 × 192 × 3 (uint8) |
| **Output Shape** | 1 × 1 × 17 × 3 (float32) — [y, x, confidence] |
| **Runtime** | ai-edge-litert (TFLite), 4 threads |
| **Python** | 3.12.3 |
| **OS** | Ubuntu Linux |

---

## 3. Input Video

| Property | Value |
|----------|-------|
| **File** | `data/raw/sample_walking.mp4` |
| **Resolution** | 1080 × 1920 (portrait) |
| **FPS** | 29.97002997 |
| **Frame Count** | 405 |
| **Duration** | ~13.5 seconds |
| **Camera** | Static position, frontal view |

---

## 4. Preprocessing Approaches

### 4.1 Direct Resize (Initial Pipeline)
- Resized full frame directly to 192×192
- Distorts aspect ratio (1080×1920 → 192×192)
- Body proportions stretched/compressed

### 4.2 Letterbox Preprocessing (Improved Pipeline, Section 20)
- Scale factor: `s = min(192/1080, 192/1920) = 0.1`
- Resized content: 108 × 192 pixels
- Horizontal padding: 42 pixels left/right
- Vertical padding: 0 pixels
- Inverse transform: `x_orig = (x_model - 42) / 0.1`, `y_orig = y_model / 0.1`

**Validation:** Center point (0.5, 0.5) correctly maps to (540, 960) — image center.

---

## 5. Frame Sampling & Inference

| Parameter | Value |
|-----------|-------|
| **Sampling Interval** | Every 10 frames (~0.334 s) |
| **Total Sampled Frames** | 41 (indices 0, 10, 20, ..., 400) |
| **Keypoints per Frame** | 17 (COCO format) |
| **Total Keypoint Predictions** | 41 × 17 = 697 |

**Pipeline:**
```
Original frame (BGR) → RGB → Letterbox resize+pad → TFLite inference → 
Inverse transform (remove pad, divide by scale) → Normalized original coords
```

---

## 6. Keypoint Coordinate Conventions

| Stage | Format | Normalization |
|-------|--------|---------------|
| Model output | [y_n, x_n, c] | Relative to 192×192 model input |
| After inverse transform | [y_n, x_n, c] | Relative to 1080×1920 original image |
| For pixel drawing | (x_px, y_px) | x_n × 1080, y_n × 1920 |

**COCO 17-keypoint indices used:**
- 11: Left hip, 13: Left knee, 15: Left ankle
- 12: Right hip, 14: Right knee, 16: Right ankle

---

## 7. Skeleton Visualization

- **Confidence threshold:** 0.3 (exploratory)
- **Edges drawn:** 19 COCO connections
- **Results:** Visually plausible skeleton on frame 0; all 17 keypoints passed confidence/boundary checks

---

## 8. Confidence Score Analysis (First 4 Seconds, 12 Frames)

| Joint | Mean Confidence | Range |
|-------|----------------|-------|
| Left hip | 0.49 | 0.38–0.60 |
| Right hip | 0.46 | 0.37–0.64 |
| Left knee | 0.40 | 0.35–0.47 |
| Right knee | 0.37 | 0.27–0.43 |
| Left ankle | 0.42 | 0.33–0.54 |
| Right ankle | 0.34 | 0.30–0.49 |

**Observations:**
- Hips most reliable (mean > 0.45)
- Right knee lowest confidence (dips to 0.27 at frame 90)
- Annotations show visible oscillations between frames

---

## 9. Knee Angle Calculation

### Method
- **Landmarks:** Left hip (11), Left knee (13), Left ankle (15)
- **Coordinates:** Pixel space (original image dimensions)
- **Formula:** θ = arccos((v₁·v₂)/(|v₁||v₂|)) where v₁ = Hip−Knee, v₂ = Ankle−Knee
- **Validity checks:** Finite coords, inside image, confidence ≥ 0.3, non-zero segments

### Results (Letterbox Pipeline)

| Metric | Value |
|--------|-------|
| **Total sampled frames** | 41 |
| **Valid angle estimates** | 18 (44%) |
| **Excluded** | 23 (confidence < 0.3, out-of-bounds, or NaN) |
| **Min angle** | 143.27° |
| **Max angle** | 179.81° |
| **Mean angle** | 169.54° |

**Valid frames:** 10, 20, 30, 40, 50, 60, 80, 90, 100, 110, 120, 130, 140, 160, 170, 180, 190, 200

---

## 10. Sudden Angle Change Investigation (2.669–3.003 s)

### Detected Anomaly
| Interval | Angle Change | Time |
|----------|--------------|------|
| 2.669 → 3.003 s | **−21.20°** | 0.334 s |
| 3.003 → 3.337 s | +13.48° | 0.334 s |

### Frames Inspected (80, 90, 100)

| Frame | Time | Left Knee Angle | Left Knee Conf | Right Knee Conf |
|-------|------|-----------------|----------------|-----------------|
| 80 | 2.669 s | 178.31° | 0.428 | 0.553 |
| 90 | 3.003 s | **157.11°** | 0.459 | **0.288** |
| 100 | 3.337 s | 170.59° | 0.558 | 0.489 |

### Visual Inspection Findings (Frames 80, 90, 100)

**Frame 80 (2.669 s):** Both legs distinguishable; left ankle slightly occluded by right leg.

**Frame 90 (3.003 s):** Left knee predicted at x=0.616 (shifted right); right knee confidence drops to 0.288; both legs overlap significantly in image.

**Frame 100 (3.337 s):** Legs separating again; predictions stabilize.

### Displacement Analysis (Frames 80→90→100)

| Joint | 80→90 Displacement | 90→100 Displacement |
|-------|-------------------|---------------------|
| Left knee | **61.9 px** | 30.6 px |
| Right knee | 50.2 px | **55.6 px** |
| Right ankle | **63.7 px** | 38.7 px |

**Interpretation:** Large left knee displacement (61.9 px) coincides with right knee confidence drop and bilateral geometry changes — consistent with occlusion/ambiguity during leg crossing.

---

## 11. Bilateral Leg Geometry Comparison

| Frame | Left Thigh | Left Shank | Left Knee | Right Thigh | Right Shank | Right Knee |
|-------|-----------|-----------|-----------|------------|------------|-----------|
| 80 | 176.3 px | 196.1 px | **178.3°** | 205.8 px | 205.9 px | 171.9° |
| 90 | 192.4 px | 188.2 px | **157.1°** | 215.4 px | 202.7 px | 167.9° |
| 100 | 208.5 px | 194.5 px | 170.6° | 214.8 px | 207.9 px | 179.8° |

**Observation:** Left knee angle drops 21° at frame 90 while right knee also decreases but less dramatically. Right thigh/shank lengths remain stable (~215/205 px) whereas left thigh length increases 16 px (176→192 px) at frame 90 — suggesting possible landmark swap or mislocalization during occlusion.

---

## 12. Temporal Consistency (Displacement Between Frames)

**Mean displacements (first 4 s, 11 intervals):**

| Joint | Mean Displacement | Maximum |
|-------|------------------|---------|
| Left hip | 20.5 px | 52.4 px |
| Right hip | 21.4 px | 57.1 px |
| Left knee | 31.2 px | 45.1 px |
| **Right knee** | **39.0 px** | **131.0 px** |
| Left ankle | 30.3 px | 54.0 px |
| Right ankle | 34.8 px | 60.6 px |

**Right knee maximum (131 px)** occurs at 3.337–3.670 s (frames 100→110), coinciding with camera movement and right knee confidence ~0.35.

---

## 13. Coordinate Validity After Letterbox Inverse Transform

| Metric | Value |
|--------|-------|
| Total predictions | 697 |
| Finite coordinates | 697/697 (100%) |
| Inside image bounds | 684/697 (98.1%) |
| Outside bounds | 13/697 (1.9%) |
| Y range | 0.333 – 1.024 |
| X range | 0.149 – 0.953 |

**Outside bounds:** Occasional predictions in horizontal padding region (x_n < 0 or x_n > 1).

---

## 14. Confidence vs. Displacement (Right Knee)

**Largest right knee displacement:** 130.95 px (3.337→3.670 s), mean confidence 0.346.

**Interpretation:** Large displacement + low confidence = unreliable prediction. Camera motion also contributes (affects all joints simultaneously).

---

## 15. Letterbox Preprocessing Validation

- **Scale factor:** 0.1 (preserves 1080:1920 = 9:16 aspect ratio)
- **Resized content:** 108 × 192
- **Padding:** 42 px left/right
- **Inverse transform test:** Center (0.5, 0.5) → (540, 960) ✓
- **Pipeline:** Re-inference on all 41 frames with letterboxing → stored in `letterboxed_keypoints` (41, 17, 3)

---

## 16. Limitations & Unresolved Questions

1. **No ground truth** — Cannot validate anatomical accuracy of keypoints or angles
2. **Frontal view limitation** — Hip/knee/ankle flexion primarily in sagittal plane; frontal view poorly captures flexion/extension
3. **Occlusion ambiguity** — Leg crossing causes landmark swaps and confidence drops; no automated occlusion detection
4. **Camera movement** — Detected in video; displaces all keypoints simultaneously, confounding displacement analysis
5. **Confidence calibration** — Threshold 0.3 is exploratory; not calibrated to anatomical accuracy
6. **2D vs 3D** — Angles are 2D image-plane projections, not true 3D joint angles
6. **Letterbox padding** — 13/697 keypoints map to padding region (x < 0 or x > 1)

---

## 16. Key Conclusions

| Finding | Evidence |
|---------|----------|
| Letterbox preprocessing works | Inverse transform correctly recovers center point |
| Knee angles plausible at straight leg | Angles ~178° during stance phase |
| Sudden angle drop at 3 s | −21° in 0.33 s, coincides with leg crossing/occlusion |
| Right knee confidence dip | 0.288 at frame 90 during leg overlap |
| Bilateral geometry changes | Left thigh length +16 px at frame 90 suggests mislocalization |
| Right knee unstable | Max displacement 131 px; confidence often < 0.4 |

**Overall:** Pipeline works for exploratory analysis. Sudden angle change at ~3 s is **likely pose-estimation instability during leg occlusion**, not true 21° knee flexion in 0.33 s. Frontal view unsuitable for sagittal-plane gait analysis.

---

## 17. Next Steps (Stage 3+)

1. **Stage 3:** Refine knee-angle time series (smoothing/interpolation), add hip/ankle angles
2. **Stage 4:** Acquire **sagittal-plane** walking video (side view) for true gait analysis
3. **Occlusion handling:** Investigate temporal smoothing, Kalman filtering, or confidence-weighted interpolation
4. **Camera stabilization** — Detect and compensate camera motion
4. **Sagittal-plane video** — Required for valid flexion/extension measurements

---

## 18. Files Generated

| File | Description |
|------|-------------|
| `notebooks/stage_2_pose_estimation.ipynb` | Complete experimental notebook (94 cells) |
| `models/movenet_lightning.tflite` | TFLite model (4.8 MB) |
| `letterboxed_keypoints` (in notebook) | (41, 17, 3) — original coords |
| `letterboxed_model_keypoints` (in notebook) | (41, 17, 3) — raw model coords |
| `letterboxed_frame_indices` | [0, 10, 20, ..., 400] |
| `letterboxed_knee_angles` | 18 valid estimates |

---

*Report generated from `notebooks/stage_2_pose_estimation.ipynb` execution outputs.*