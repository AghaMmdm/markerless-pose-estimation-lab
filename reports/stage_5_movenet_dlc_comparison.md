# Stage 5: MoveNet vs DeepLabCut Comparison — Experimental Report

**Date:** 2026-11-05  
**Experiment ID:** exp_002  
**Status:** COMPLETED  
**Notebook:** `notebooks/stage_4_deeplabcut_fundamentals.ipynb` (cells 19–35, comparison section)  
**Comparison outputs:** `data/processed/comparison/`

---

## 1. Objective

Compare the consistency of MoveNet and DeepLabCut pose-estimation predictions on the same sagittal-plane walking video. The goal is to evaluate whether the two models produce consistent lower-limb keypoint trajectories and knee-angle estimates, and to understand where they agree or diverge.

**This comparison does not establish ground-truth accuracy** — neither model's predictions are independently validated. Agreement between models is not the same as correctness.

---

## 2. Environment & Models

| Property | MoveNet Lightning | DeepLabCut (SuperAnimal-Human) |
|----------|-------------------|--------------------------------|
| **Model** | MoveNet Lightning (TFLite) | SuperAnimal-Human / RTMPose-X |
| **Detector** | — | Faster R-CNN MobileNet V3 Large FPN |
| **Inference type** | Direct | Top-down (person detection → pose estimation) |
| **Input shape** | 1 × 192 × 192 × 3 (uint8) | Internal DLC preprocessing |
| **Output shape** | 1 × 1 × 17 × 3 (float32) | MultiIndex DataFrame: scorer × individuals × bodyparts × coords |
| **Runtime** | ai-edge-litert (TFLite), 4 threads | DeepLabCut 3.0.2, PyTorch 2.14.1+cu126 |
| **Device** | CPU | CUDA (NVIDIA RTX 4060 Laptop GPU) |
| **Video adaptation** | N/A | `video_adapt=False` (zero-shot) |
| **Python** | 3.12.3 | 3.12.3 (separate `.venv-dlc` env) |

---

## 3. Input Video

| Property | Value |
|----------|-------|
| **File** | `data/raw/sagittal_walking.mp4` |
| **Resolution** | 1920 × 1080 (landscape, sagittal view) |
| **FPS** | 30.0 |
| **Frame Count** | 406 |
| **Duration** | ~13.5 seconds |
| **Camera** | Side view (sagittal plane) |

---

## 4. Frame Sampling & Alignment

| Parameter | Value |
|-----------|-------|
| **MoveNet sampling** | Every 5th frame |
| **MoveNet sampled frames** | 82 (indices 0, 5, 10, ..., 405) |
| **DeepLabCut frames** | 406 (every frame) |
| **Comparison frames** | 82 (MoveNet indices, matched to DLC) |
| **Keypoints per frame** | 6 lower-limb (left/right hip, knee, ankle) |

**Alignment strategy:** Only MoveNet-sampled frame indices are compared against DLC predictions at the same indices. DLC frames at non-multiples of 5 are excluded from the comparison. This ensures paired observations at identical time points.

---

## 5. Keypoint Coordinate Conventions

| Stage | Format | Normalization |
|-------|--------|---------------|
| MoveNet model output | [y_n, x_n, c] | Relative to 192×192 model input |
| MoveNet after inverse transform | [y_n, x_n, c] | Relative to 1920×1080 original image |
| MoveNet for angle calculation | [x_px, y_px] | x_n × 1920, y_n × 1080 |
| DeepLabCut output | [x, y, likelihood] | Original image pixel space (1920×1080) |

**MoveNet → pixel conversion:** `x_px = x_norm × 1920`, `y_px = y_norm × 1080`  
**DeepLabCut → pixel conversion:** Direct (already in pixel space)

**COCO 17-keypoint indices used:**
- 11: Left hip, 12: Right hip
- 13: Left knee, 14: Right knee
- 15: Left ankle, 16: Right ankle

**DLC bodypart names:** `left_hip`, `right_hip`, `left_knee`, `right_knee`, `left_ankle`, `right_ankle`

---

## 6. Confidence Filtering

| Model | Threshold | Invalid Value |
|-------|-----------|---------------|
| MoveNet | 0.3 | < 0.3 → NaN |
| DeepLabCut | 0.3 | < 0 or < 0.3 → NaN |

**Important:** MoveNet confidence scores and DeepLabCut likelihood values are **not directly calibrated** and should not be treated as comparable probability values. A MoveNet confidence of 0.5 does not mean the same thing as a DLC likelihood of 0.5. Compare confidence within each model, not across models.

---

## 7. Knee Angle Calculation

### Method
- **Landmarks:** Hip (11/12), Knee (13/14), Ankle (15/16)
- **Coordinates:** Pixel space (1920 × 1080)
- **Formula:** θ = arccos((v₁·v₂)/(|v₁||v₂|)) where v₁ = Hip−Knee, v₂ = Ankle−Knee
- **Validity checks:** Finite coords, non-NaN, confidence ≥ 0.3 for all three landmarks, non-zero segment lengths

### Results

| Metric | Left Knee | Right Knee |
|--------|-----------|------------|
| **Valid paired frames** | 51 | 71 |
| **MAE (degrees)** | 20.944 | 9.341 |
| **RMSE (degrees)** | 26.099 | 12.493 |
| **Pearson correlation** | −0.073 | 0.732 |
| **Mean bias (degrees)** | 4.363 | 0.309 |
| **Difference std (degrees)** | 25.987 | 12.578 |
| **Min difference (degrees)** | −33.934 | −27.261 |
| **Max difference (degrees)** | 55.784 | 30.496 |

---

## 8. Detection Coverage

| Model | Left Knee | Right Knee |
|-------|-----------|------------|
| **MoveNet** | 53/82 (65%) | 71/82 (87%) |
| **DeepLabCut** | 80/82 (98%) | 82/82 (100%) |

**Observations:**
- DeepLabCut detects the left knee in more sampled frames than MoveNet (80 vs 53)
- MoveNet misses 29 frames for left knee (mostly NaN due to confidence < 0.3)
- DeepLabCut detects all 82 frames for right knee
- MoveNet detects 71/82 frames for right knee

---

## 9. Interpretation

### Model Agreement vs Accuracy

The two models showed substantially better agreement for the right knee:
- Right-knee correlation: **0.732** (strong positive correlation)
- Left-knee correlation: **−0.073** (essentially no correlation)
- Right-knee MAE: **9.34°** (moderate agreement)
- Left-knee MAE: **20.94°** (substantial disagreement)

**Key finding:** Model agreement ≠ measurement accuracy. If both models produce similar angles, it means they agree with each other, not that either is anatomically correct. Independent ground-truth measurements (e.g., motion capture) would be required to establish accuracy.

### Occlusion Analysis

Left-leg occlusion — caused by the leg being behind the right leg — likely contributed to the weaker left-knee results:
- MoveNet left-knee confidence drops significantly during leg overlap
- DeepLabCut maintains higher detection rates for left knee (80/82 vs MoveNet's 53/82)
- Large individual angle differences remain even where both models agree on detection

### Bias Analysis

- Right-knee mean bias: **0.31°** (close to zero, indicating systematic agreement)
- Left-knee mean bias: **4.36°** (MoveNet tends to predict larger angles)
- Left-knee difference std: **25.99°** (high variability between models)
- Right-knee difference std: **12.58°** (moderate variability)

---

## 10. Limitations

1. **Neither model is ground truth** — MoveNet and DeepLabCut are both predictive models. Their agreement or disagreement cannot establish anatomical accuracy without an independent reference (e.g., motion capture, carefully established reference annotations).

2. **Pretrained DLC model limitations** — The SuperAnimal-Human pretrained model provides fixed COCO-style keypoints. It does not automatically predict custom anatomical landmarks. Custom landmark definition would require manual annotation and model training (a future Stage 6 activity).

3. **Different preprocessing** — MoveNet uses letterbox preprocessing with aspect-ratio preservation and coordinate inverse transform. DLC SuperAnimal-Human uses its own internal preprocessing (person detection → pose estimation). These differences can affect keypoint localization independently of model quality.

4. **Single video, single subject** — Results are specific to this sagittal walking video and may not generalize to other videos, subjects, or camera setups.

5. **2D only** — Both models produce 2D image-plane coordinates. Angles are 2D projections and may not reflect true 3D joint angles.

6. **Confidence scores are not calibrated across models** — MoveNet confidence and DLC likelihood use different scales and should not be directly compared.

---

## 11. Key Conclusions

| Finding | Evidence |
|---------|----------|
| Right knee shows strong model agreement | Correlation 0.732, MAE 9.34°, bias 0.31° |
| Left knee shows weak model agreement | Correlation −0.073, MAE 20.94°, bias 4.36° |
| DeepLabCut has higher detection coverage | 80/82 left knee vs MoveNet 53/82 |
| Agreement ≠ accuracy | No independent ground truth available |
| Occlusion affects left knee more | Left leg behind right leg during parts of gait cycle |

**Overall:** The two models show meaningful agreement for right-knee angles but substantial disagreement for left-knee angles, likely due to occlusion during leg crossing. DeepLabCut provides more consistent detection coverage, particularly for the left knee. Neither model's output should be treated as anatomically accurate without independent validation.

---

## 12. Next Steps (Stage 6+)

1. **Stage 6: Research experiments with alternative anatomical landmarks** — Investigate whether adding anatomical landmarks beyond hip–knee–ankle can improve knee-angle estimation (thigh/shank segment orientations, additional knee landmarks).
2. **Custom DeepLabCut training** — If custom landmarks are needed, manually annotate frames and train a DLC model (requires supervisor approval on landmark configuration).
3. **Independent validation** — Obtain motion-capture or other ground-truth data to evaluate actual accuracy of both models.
4. **Temporal smoothing** — Apply Kalman filtering or EMA to angle time series to reduce frame-to-frame jitter.

---

## 13. Files Generated

| File | Description |
|------|-------------|
| `notebooks/stage_4_deeplabcut_fundamentals.ipynb` | Complete notebook including comparison section (cells 19–35) |
| `data/processed/comparison/movenet_dlc_knee_angles.csv` | Paired knee angles for all 82 sampled frames |
| `data/processed/comparison/agreement_metrics.csv` | MAE, RMSE, Pearson correlation per knee |
| `data/processed/comparison/bias_analysis.csv` | Mean bias, std, min/max difference per knee |
| `data/processed/comparison/detection_coverage.csv` | Valid detection counts per model per knee |
| `data/processed/deeplabcut/*.h5` | DLC HDF5 predictions (406 frames) |
| `data/processed/deeplabcut/*.json` | DLC JSON metadata |
| `data/processed/deeplabcut/*_labeled_before_adapt.mp4` | DLC labeled video |

---

*Report generated from `notebooks/stage_4_deeplabcut_fundamentals.ipynb` execution outputs (comparison cells 19–35).*