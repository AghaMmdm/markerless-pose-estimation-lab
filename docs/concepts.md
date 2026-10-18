# Concepts Glossary

A living document of key terms in markerless pose estimation. Expand as you learn.

---

## Markerless Pose Estimation
**Definition:** Computer vision task of detecting human body keypoints (joints, landmarks) from images or video without physical markers attached to the body.
**Context:** Contrasts with marker-based motion capture (mocap) which requires reflective markers and controlled environments.

---

## Keypoint
**Definition:** A specific anatomical landmark detected on the body, represented as (x, y) coordinates in 2D or (x, y, z) in 3D, often with an associated confidence score.
**Examples:** Nose, left eye, right shoulder, left knee, right ankle.
**Standard sets:** COCO 17-keypoint, MPII 16-keypoint, OpenPose BODY_25 (25 keypoints).

---

## Skeleton
**Definition:** A predefined set of connections (edges) between keypoints that represents the human body structure.
**Purpose:** Visualization, biomechanical analysis, constraint enforcement.
**Example (COCO):** Nose→Left Eye, Left Eye→Left Ear, Left Shoulder→Left Elbow→Left Wrist, etc.

---

## Confidence Score
**Definition:** A probability-like value (typically 0.0–1.0) output by the model indicating how certain it is that a keypoint exists at the predicted location.
**Usage:** Filtering (threshold), weighting in loss functions, uncertainty visualization.
**Note:** Not calibrated probability; model-dependent.

---

## Bounding Box
**Definition:** Axis-aligned rectangle (x_min, y_min, x_max, y_max) enclosing a detected person.
**Role:** Input to single-person pose estimators (crop → resize → infer); output of person detectors in multi-person pipelines.

---

## Frame Sampling
**Definition:** Processing a subset of video frames (e.g., every Nth frame, keyframes only) to reduce computation.
**Trade-off:** Speed vs temporal resolution; affects motion analysis quality.
**Strategies:** Uniform, adaptive (scene change), importance-based.

---

## Temporal Consistency
**Definition:** The property that keypoint predictions vary smoothly across consecutive frames without jitter or sudden jumps.
**Importance:** Critical for angle calculation, motion analysis, action recognition.
**Improvement methods:** Kalman filters, exponential smoothing, temporal CNNs, optical flow.

---

## Joint Angle
**Definition:** The angle (in degrees or radians) formed by three keypoints representing adjacent body segments (e.g., hip-knee-ankle for knee flexion).
**Calculation:** 2D angle in image plane (projection) or 3D angle in world coordinates.
**Limitation:** 2D angles are view-dependent; 3D requires calibrated multi-view or depth.

---

## Ground Truth
**Definition:** Annotated reference data (keypoint locations) used for training supervision or evaluation.
**Sources:** Manual annotation, marker-based mocap synchronization, synthetic data.
**Quality:** Inter-annotator agreement, precision of annotation tool.

---

## Evaluation Metric
**Definition:** Quantitative measure of pose estimation accuracy.
**Common metrics:**
- **PCK (Percentage of Correct Keypoints):** Fraction of keypoints within normalized distance threshold of ground truth.
- **OKS (Object Keypoint Similarity):** COCO-standard metric, analogous to IoU for keypoints.
- **AP (Average Precision):** Area under precision-recall curve across OKS thresholds.
- **MPJPE (Mean Per Joint Position Error):** Mean Euclidean distance (pixels or mm).
- **AUC (Area Under Curve):** PCK integrated over threshold range.

---

## Data Leakage
**Definition:** Information from test/validation set inadvertently used during training or model selection, inflating performance estimates.
**Examples:** Same video in train and test, frames from same sequence split across sets, using test metrics for hyperparameter tuning.
**Prevention:** Split by subject/video, not by frame; strict temporal separation.

---

## Reproducibility
**Definition:** Ability to obtain identical (or statistically equivalent) results when re-running an experiment with the same code, data, and environment.
**Requirements:** Fixed random seeds, versioned dependencies, documented hardware, complete configuration, containerization (Docker) or environment files.

---

## COCO Keypoint Format
**Definition:** Standard 17-keypoint format used by COCO dataset and many models (MoveNet, PoseNet, etc.).
**Keypoints (0-indexed):** 0:Nose, 1:Left Eye, 2:Right Eye, 3:Left Ear, 4:Right Ear, 5:Left Shoulder, 6:Right Shoulder, 7:Left Elbow, 8:Right Elbow, 9:Left Wrist, 10:Right Wrist, 11:Left Hip, 12:Right Hip, 13:Left Knee, 14:Right Knee, 15:Left Ankle, 16:Right Ankle.
**Skeleton edges:** 19 connections defined by COCO.

---

## Part Affinity Fields (PAFs)
**Definition:** 2D vector fields encoding limb orientation and association, used by OpenPose for multi-person pose estimation.
**Mechanism:** Each limb has a PAF channel; keypoints are associated by maximizing PAF alignment along the limb.

---

## Transfer Learning
**Definition:** Using a pre-trained model (on large dataset like COCO, ImageNet) as starting point for a new task (custom keypoints, new domain).
**Approaches:** Fine-tuning all layers, freezing backbone + training head, adapter layers.
**DeepLabCut:** Specialized transfer learning pipeline for pose estimation with limited data.

---

## Inference Pipeline
**Definition:** End-to-end sequence: video → frame extraction → preprocessing (resize, normalize) → model inference → postprocessing (NMS, grouping) → coordinate mapping → output (keypoints, video, CSV).
**Optimization:** Batching, asynchronous I/O, model quantization, tensorRT/ONNX.

---

## Quantization
**Definition:** Reducing model weight precision (FP32 → INT8) for faster inference and smaller size.
**Types:** Post-training quantization (PTQ), quantization-aware training (QAT).
**Impact:** Usually <1-2% accuracy drop for pose estimation; significant speedup on edge devices.

---

## Optical Flow
**Definition:** Pixel-wise motion vectors between consecutive frames.
**Use in pose estimation:** Keypoint tracking, temporal smoothing, frame interpolation, motion-guided sampling.

---

## Domain Gap
**Definition:** Performance degradation when model trained on one domain (e.g., COCO: diverse internet images) is applied to another (e.g., lab videos, specific clothing, unusual camera angles).
**Mitigation:** Fine-tuning, data augmentation, domain adaptation, synthetic data.

---

## Keypoint Visibility / Occlusion
**Definition:** Whether a keypoint is visible in the image. COCO uses: 0=not labeled, 1=labeled but occluded, 2=visible.
**Handling:** Models predict confidence; low confidence often indicates occlusion. Some architectures explicitly predict visibility.

---

## Single-Person vs Multi-Person
**Single-Person:** Assumes one dominant person (MoveNet, PoseNet single-pose). Faster, simpler.
**Multi-Person:** Detects and associates keypoints for multiple people (OpenPose, MoveNet MultiPose, YOLO+HRNet). Needs person detection + keypoint grouping.

---

## Top-Down vs Bottom-Up
**Top-Down:** Person detector → crop each person → single-person pose estimator. (HRNet, SimpleBaseline, AlphaPose). Accurate, slower with many people.
**Bottom-Up:** Detect all keypoints → group into persons (OpenPose, HigherHRNet, DEKR). Faster for crowds, harder occlusion handling.

---

## Heatmap vs Regression
**Heatmap-based:** Model outputs probability map per keypoint; argmax gives location. (HRNet, SimpleBaseline, OpenPose). Sub-pixel accuracy via soft-argmax.
**Regression-based:** Model directly outputs (x, y) coordinates. (MoveNet, PoseNet, DirectPose). Faster, lighter, but historically less accurate.

---

## Model Input Size
**Definition:** Fixed resolution (e.g., 192×192, 256×256, 512×512) that the model expects. Input frames are resized/padded to this size.
**Trade-off:** Larger = more detail, slower, more memory. MoveNet Lightning: 192×192; Thunder: 256×256.

---

## Non-Maximum Suppression (NMS)
**Definition:** Algorithm to remove duplicate detections (bounding boxes or keypoints) by keeping highest-scoring and suppressing overlapping lower-scoring ones.
**OKS-NMS:** Uses OKS instead of IoU for keypoint-based NMS in multi-person.

---

*Add new terms as you encounter them in papers, documentation, and experiments.*