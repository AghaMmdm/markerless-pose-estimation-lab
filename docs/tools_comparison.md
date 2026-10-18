# Tools Comparison: MoveNet, PoseNet, OpenPose, DeepLabCut

Based on official documentation and community resources. Updated as experiments progress.

---

## Overview Table

| Aspect | MoveNet | PoseNet | OpenPose | DeepLabCut |
|--------|---------|---------|----------|------------|
| **Primary use in this project** | Stage 2: First lightweight baseline | Stage 4: Classic architecture comparison | Stage 5: Multi-person reference | Stage 6: Custom keypoint training |
| **Model type** | Single-person (Lightning/Thunder), MultiPose variant | Single-person & multi-person | Multi-person (bottom-up) | Custom keypoint detector (transfer learning) |
| **Keypoints** | 17 (COCO) | 17 (COCO) | 25 (BODY_25), 135 (with hands/face) | User-defined (any number) |
| **Architecture** | MobileNetV3 + custom head (regression) | ResNet50/MobileNetV0.75 + heatmap + PAF-like | VGG/ResNet + PAFs + heatmaps | ResNet-50/101/MobileNetV2 + DeepLabCut head |
| **Input resolution** | 192×192 (Lightning), 256×256 (Thunder) | 257×257 (default), configurable | 368×368 (default), scalable | Configurable (default 224×224–512×512) |
| **Speed (CPU)** | ~30-50 FPS (Lightning) | ~5-15 FPS (ResNet50) | ~1-5 FPS (CPU, BODY_25) | Training: hours; Inference: ~10-30 FPS |
| **Speed (GPU)** | ~100+ FPS | ~50+ FPS | ~20-30 FPS | ~50-100+ FPS |
| **Multi-person** | MultiPose variant (separate model) | Multi-person (decode multiple poses) | Native (designed for crowds) | Single-animal/person per video (multi-animal variant exists) |
| **Pretrained weights** | TensorFlow Hub, TFLite | TensorFlow.js, TF Hub, Keras | Caffe, PyTorch, ONNX | Pre-trained on mouse/human datasets |
| **Custom training** | Not officially supported (fine-tuning possible but not documented) | Not officially supported | Not supported (fixed architecture) | **Core feature** - designed for custom keypoints |
| **License** | Apache 2.0 | Apache 2.0 (TF.js), MIT (ports) | Non-commercial research only (commercial license required) | GNU GPL v3 (DeepLabCut), BSD-3 (DLC-live) |
| **Framework** | TensorFlow / TF.js / TFLite | TensorFlow.js, TensorFlow, Keras | Caffe, PyTorch | TensorFlow, PyTorch (DLC 2.3+) |
| **Python API** | TF Hub, TFLite Interpreter | Keras/TF, TF.js (JS) | Python wrapper (OpenPose Python) | Native Python package |
| **Ease of setup (CPU)** | ⭐⭐⭐⭐⭐ Easy (pip install TF, TF Hub) | ⭐⭐⭐ Medium (TF.js → Python ports vary) | ⭐⭐ Hard (Caffe build, dependencies) | ⭐⭐⭐ Medium (conda env, GPU recommended for training) |
| **Documentation quality** | Good (TF Hub, Colabs) | Good (TF.js), Variable (Python ports) | Moderate (README, papers) | Excellent (extensive docs, tutorials) |
| **Community/Support** | Active (Google Research) | Active (TF.js team), many forks | Large (CMU, many forks) | Active (Mathis Lab, MW lab, growing) |

---

## Detailed Comparison

### MoveNet (Google Research)

**Official:** https://tfhub.dev/google/movenet/singlepose/lightning/4

**Variants:**
- **Lightning:** 192×192, fastest, ~3.2M params, good for real-time/CPU
- **Thunder:** 256×256, more accurate, ~6.4M params, better for quality

**Strengths:**
- Extremely lightweight and fast on CPU
- Official TensorFlow Hub integration
- TFLite models for mobile/edge
- Clean Python API via TF Hub
- Apache 2.0 license (commercial-friendly)

**Limitations:**
- Single-person only (separate MultiPose model exists but less documented)
- Fixed 17 COCO keypoints (no hands, face, feet detail)
- No official fine-tuning guide
- Regression-based (no heatmaps → harder to diagnose failures)
- Confidence scores not well-calibrated

**Intended use in this project:** Stage 2 baseline. Fast iteration on CPU. Establish keypoint extraction pipeline.

---

### PoseNet (TensorFlow.js Team)

**Official:** https://github.com/tensorflow/tfjs-models/tree/master/posenet

**Variants:**
- **ResNet50:** Higher accuracy, slower (~23M params)
- **MobileNetV1 1.0 / 0.75 / 0.50:** Speed/accuracy tradeoff

**Strengths:**
- Classic, well-known architecture (heatmap + offset vectors)
- Runs in browser (TF.js) and Python (via Keras/TF ports)
- Multi-person decoding built-in
- Good educational value (heatmap visualization)

**Limitations:**
- Python ecosystem fragmented (multiple unofficial ports)
- Slower than MoveNet on CPU
- Original TF.js model; Python ports may differ in accuracy
- Less actively maintained in Python

**Intended use in this project:** Stage 4 comparison. Understand heatmap-based approach. Educational contrast to regression-based MoveNet.

---

### OpenPose (CMU Perceptual Computing Lab)

**Official:** https://github.com/CMU-Perceptual-Computing-Lab/openpose

**Key Innovation:** Part Affinity Fields (PAFs) for bottom-up multi-person association.

**Keypoint Sets:**
- **BODY_25:** 25 body keypoints (includes feet, more spine detail)
- **COCO:** 18 keypoints (compatible)
- **MPI:** 15 keypoints
- **Hands:** 21 per hand
- **Face:** 70 keypoints
- **Total:** 135 (body+face+hands)

**Strengths:**
- Gold standard for multi-person
- Rich keypoint set (hands, face, feet)
- Robust to occlusion and crowding
- C++ implementation very fast on GPU
- Extensive literature, benchmarks

**Limitations:**
- **License:** Non-commercial research only (commercial use requires license)
- Complex setup: Caffe, CUDA, cuDNN, OpenCV build from source
- Python bindings unofficial/maintenance varies
- Heavy dependencies, large model files
- No custom keypoint training

**Intended use in this project:** Stage 5 reference. Multi-person capability demonstration. Compare keypoint richness. Document setup challenges.

---

### DeepLabCut (Mathis Lab / Mackenzie Lab)

**Official:** https://www.deeplabcut.org/ | https://github.com/DeepLabCut/DeepLabCut

**Core Concept:** Transfer learning from ImageNet/COCO pre-trained networks to user-defined keypoints with minimal labeled data (50-200 frames).

**Key Features:**
- Custom keypoint definition (any animal, any markers)
- Active learning (iterative refinement)
- 3D pose from multi-camera (DeepLabCut-3D)
- DLC-Live for real-time inference
- Extensive analysis toolbox (trajectories, kinematics)

**Strengths:**
- **Designed for custom keypoints** - unique among these tools
- Works with very little labeled data
- Strong scientific validation (neuroscience, behavior)
- Active development, excellent documentation
- Python-native, good Jupyter integration

**Limitations:**
- **License:** GPL v3 (viral - affects derivative works)
- Training requires GPU (hours for good results)
- Single-subject focus (multi-animal variant separate)
- Heavier dependency stack (TensorFlow, imgaug, etc.)
- Overkill for standard human COCO keypoints

**Intended use in this project:** Stage 6. Learn custom keypoint training workflow. Define custom keypoints (e.g., shoe markers, specific joint centers). Understand transfer learning for pose.

---

## Decision Framework for This Project

| If you need... | Start with... |
|----------------|---------------|
| Fast CPU baseline, standard human keypoints | MoveNet Lightning |
| Understanding heatmap-based methods | PoseNet (ResNet50) |
| Multi-person, hands, face, feet | OpenPose (if license permits) |
| Custom keypoints (non-standard, animals, markers) | DeepLabCut |
| Real-time mobile/edge deployment | MoveNet TFLite |
| Browser-based demo | MoveNet TF.js / PoseNet TF.js |
| Scientific publication with custom markers | DeepLabCut |

---

## Practical Limitations to Document

### MoveNet
- [ ] Confidence threshold tuning needed
- [ ] Single-person failure modes (occlusion, truncation)
- [ ] 2D only (no depth)

### PoseNet
- [ ] Python port accuracy vs original TF.js
- [ ] Multi-person decoding parameters

### OpenPose
- [ ] License compliance for this educational project
- [ ] Build complexity on Ubuntu
- [ ] Model file sizes (~100MB+)

### DeepLabCut
- [ ] GPU requirement for training
- [ ] Labeling effort (even with active learning)
- [ ] GPL license implications for sharing code

---

## License Compliance Checklist

- [ ] MoveNet: Apache 2.0 ✓ (commercial OK, include notice)
- [ ] PoseNet: Apache 2.0 / MIT ✓
- [ ] OpenPose: **Non-commercial research only** - verify educational use qualifies
- [ ] DeepLabCut: GPL v3 - any derivative work must be GPL v3

*Note: This is an educational project, not commercial. However, if you publish code on GitHub, license compatibility matters.*

---

## Version-Specific Notes

| Tool | Version Tested | Date | Notes |
|------|----------------|------|-------|
| MoveNet | TF Hub v4 | TBD | |
| PoseNet | TF.js 2.4.0 / Keras port | TBD | |
| OpenPose | 1.7.0 | TBD | |
| DeepLabCut | 2.3.x | TBD | |

*Update after each experiment.*

---

## References

1. MoveNet: "MoveNet: Ultra Fast and Accurate Pose Detection" (Google Research, 2021)
2. PoseNet: "Real-time Human Pose Estimation in the Browser with TensorFlow.js" (TF.js Team, 2018)
3. OpenPose: "OpenPose: Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields" (Cao et al., CVPR 2017)
4. DeepLabCut: "DeepLabCut: Markerless pose estimation of user-defined body parts with deep learning" (Mathis et al., Nature Neuroscience 2018)

---

*This comparison reflects documentation at project start. Update with empirical findings from experiments.*