# Research Notes

Ongoing observations, questions, and findings during the learning project.

---

## 2024-XX-XX: Project Initialization

**Context:** Setting up the initial scaffold for the markerless pose estimation learning project.

**Decisions Made:**
- Project structure follows data science conventions (data/, notebooks/, src/, experiments/, reports/, docs/)
- Lightweight dependencies only (OpenCV, NumPy, Pandas, Matplotlib, Jupyter)
- MoveNet chosen as first model (CPU-friendly, Apache 2.0, TF Hub integration)
- Sample video from Pexels (permissive license, single person walking)

**Open Questions:**
- [ ] How does MoveNet confidence threshold affect keypoint quality?
- [ ] What's the best way to handle missing keypoints in angle calculation?
- [ ] How to evaluate without ground truth?
- [ ] When to switch from MoveNet to PoseNet/OpenPose/DeepLabCut?

---

## Template for Future Entries

### YYYY-MM-DD: [Topic/Experiment]

**Context:** What prompted this note?

**Observation:** What did you see/measure?

**Question:** What are you uncertain about?

**Action:** What will you try next?

**References:** Links to papers, docs, issues

---

## Experiment-Specific Notes

### exp_001: MoveNet Baseline
- *To be filled after experiment*

### exp_002: PoseNet Comparison
- *To be filled after experiment*

### exp_003: OpenPose Evaluation
- *To be filled after experiment*

### exp_004: DeepLabCut Custom Training
- *To be filled after experiment*

---

## Literature to Read

- [ ] MoveNet paper / blog post
- [ ] PoseNet architecture details
- [ ] OpenPose PAF paper (Cao et al., CVPR 2017)
- [ ] DeepLabCut paper (Mathis et al., Nature Neuroscience 2018)
- [ ] COCO keypoint evaluation methodology
- [ ] PCK/OKS metric implementations
- [ ] Temporal smoothing for pose (Kalman, EMA, TCN)
- [ ] 2D to 3D pose lifting (VideoPose3D, etc.)

---

## Ideas for Extensions

1. **Multi-video comparison:** Same person, different cameras/clothing
2. **Synthetic data:** Use Blender/Unity to generate ground truth
3. **Action classification:** Walk vs run vs jump from keypoints
4. **Gait asymmetry:** Left/right leg angle comparison
5. **Real-time demo:** Webcam + MoveNet TFLite
6. **Mobile deployment:** Convert to TFLite, test on phone

---

## Challenges Encountered

| Date | Challenge | Resolution | Notes |
|------|-----------|------------|-------|
|      |           |            |       |

---

## Useful Code Snippets

*Collect reusable patterns here*

```python
# Example: Safe keypoint access with confidence check
def get_keypoint(keypoints, name, threshold=0.3):
    """Get keypoint if confidence > threshold, else None."""
    idx = KEYPOINT_NAMES.index(name)
    kp = keypoints[idx]
    if kp[2] > threshold:
        return kp[:2]  # x, y
    return None
```

---

*Update regularly - this is your research lab notebook.*