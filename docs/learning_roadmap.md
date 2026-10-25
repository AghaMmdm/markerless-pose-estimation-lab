# Learning Roadmap (4 Weeks)

Approximate timeline: 1-2 hours/day, 4 weeks total.

---

## Week 1: Video Processing Fundamentals

### Learning Objectives
- Understand video file structure (containers, codecs, frames)
- Work with FPS, resolution, and frame timing
- Master OpenCV VideoCapture and VideoWriter
- Read, process, and save video frames

### Topics
1. **Video files and frames**
   - Container formats (MP4, AVI, MOV)
   - Codecs (H.264, H.265, MJPEG)
   - Frame structure: BGR vs RGB
   
2. **FPS and resolution**
   - Frame rate concepts
   - Resolution impact on processing
   - Aspect ratio handling
   
3. **OpenCV VideoCapture**
   - Opening video files and streams
   - Reading frames sequentially
   - Seeking to specific frames
   - Getting video properties (FPS, frame count, dimensions)
   
4. **Reading and saving frames**
   - Frame extraction to images
   - Annotated video writing
   - Performance considerations

### Expected Outputs
- [x] Notebook: `notebooks/stage_1_video_processing.ipynb` - Interactive exploration (completed)
- [ ] Script: `scripts/extract_frames.py` - Extract frames at configurable intervals
- [ ] Script: `scripts/video_info.py` - Print video metadata
- [ ] Config: `configs/video.yaml` - Default video processing parameters
- [x] Extracted frames in `data/interim/extracted_frames/` (14 frames from timestamp and interval sampling)

### Key OpenCV Functions to Learn
- `cv2.VideoCapture()`
- `cap.read()`, `cap.set()`, `cap.get()`
- `cv2.VideoWriter()`
- `cv2.cvtColor()` (BGR↔RGB)
- `cv2.resize()`, `cv2.imwrite()`

### Mini-Exercises
1. ~~Open sample video and print: FPS, frame count, resolution, duration~~ ✅ Done (Cells 1–6, 21–22)
2. ~~Extract every 30th frame as JPEG to `data/interim/frames/`~~ ✅ Done (Cells 9–10, saved to `data/interim/extracted_frames/`)
3. ~~Create annotated video with frame numbers drawn on each frame~~ ✅ Done (Cells 18–19, 23–24)
4. ~~Measure time to process full video~~ ⏳ Not explicitly timed, but frame-by-frame processing verified

### Resources
- [OpenCV Video I/O Tutorial](https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html)
- [FFmpeg for video preprocessing](https://ffmpeg.org/ffmpeg.html)

---

## Week 2: Pose Estimation with MoveNet

### Learning Objectives
- Understand keypoints and skeleton representations
- Run MoveNet inference on video frames
- Interpret confidence scores
- Visualize pose predictions

### Topics
1. **Keypoints and skeletons**
   - COCO 17-keypoint format
   - Skeleton connectivity (edges)
   - Keypoint coordinates: normalized vs pixel
   
2. **MoveNet input and output**
   - Model variants: Lightning vs Thunder
   - Input preprocessing (resize, normalize, pad)
   - Output format: [1, 1, 17, 3] → (y, x, confidence)
   - TensorFlow Hub loading
   
3. **Confidence scores**
   - Per-keypoint confidence
   - Person detection threshold
   - Handling low-confidence keypoints
   
4. **Visualizing predictions**
   - Drawing keypoints on frames
   - Drawing skeleton connections
   - Color-coding by confidence
   - Creating annotated output video

### Expected Outputs
- [x] Notebook: `notebooks/stage_2_pose_estimation.ipynb` (completed, 94 cells)
- [x] Config: `configs/movenet.yaml` (not needed; TFLite model params in notebook)
- [x] Keypoints in original coords: `letterboxed_keypoints` (41, 17, 3) in notebook
- [x] Experiment log entry in `reports/experiment_log.md` (exp_001)
- [x] Detailed report: `reports/stage_2_pose_estimation.md`

### Key Concepts
- TFLite model loading with ai-edge-litert
- Letterbox preprocessing (preserve aspect ratio)
- Inverse coordinate transformation (model → original image)
- Keypoint confidence analysis & temporal consistency
- Geometric angle calculation from 2D keypoints
- Occlusion detection via confidence + displacement

### Mini-Exercises (Completed in Notebook)
1. ~~Load MoveNet Lightning from TF Hub~~ ✅ (Cell 41)
2. ~~Run inference on single frame, visualize output~~ ✅ (Cells 43–46)
3. ~~Process full video, save keypoints~~ ✅ (Cells 47–50, 89)
4. ~~Create annotated video with skeleton overlay~~ ✅ (Cell 90)
5. ~~Plot confidence distributions per keypoint~~ ✅ (Cell 81)
6. ~~Calculate knee angles across frames~~ ✅ (Cells 65–72)
6. ~~Investigate sudden angle changes~~ ✅ (Cells 94–96)
7. ~~Letterbox preprocessing with inverse transform~~ ✅ (Cells 86–93)

### Resources
- [MoveNet on TF Hub](https://tfhub.dev/google/movenet/singlepose/lightning/4)
- [MoveNet TFLite](https://www.tensorflow.org/lite/models/pose_estimation/overview)
- [COCO Keypoint Format](https://cocodataset.org/#keypoints-2020)

---

## Week 3: Keypoint Analysis and Motion Visualization (Stage 3)

### Learning Objectives
- Store keypoints in structured Pandas DataFrames
- Calculate joint angles from 2D keypoints (hip, knee, ankle)
- Create time-series visualizations with confidence bands
- Handle missing and low-confidence keypoints (interpolation, smoothing)
- **Investigate occlusion handling strategies**
- **Validate sagittal-plane gait analysis requirements**

### Prerequisites from Stage 2
- [x] MoveNet inference pipeline with letterbox preprocessing
- [x] Knee angle calculation function (`calculate_angle`)
- [x] Temporal displacement analysis framework
- [x] Confidence-based filtering approach

### Topics
1. **Storing keypoints in Pandas**
   - Long vs wide format
   - Multi-index: (frame, keypoint)
   - Columns: x, y, confidence, visibility
   - Saving/loading CSV efficiently
   
2. **Joint-angle calculations**
   - 2D angle between three points
   - Relevant angles: knee, hip, elbow, shoulder, ankle
   - Handling occluded keypoints
   - Smoothing noisy angle estimates
   
3. **Time-series plots**
   - Joint angle over time
   - Confidence over time
   - Trajectory plots (x, y over time)
   - Multi-panel figures
   
4. **Missing and low-confidence keypoints**
   - Interpolation strategies
   - Gap detection
   - Flagging unreliable frames
   - Effect on angle calculations

### Expected Outputs
- [ ] Notebook: `notebooks/stage_3_keypoint_analysis.ipynb`
- [ ] Module: `src/analysis/angles.py`
- [ ] Module: `src/analysis/smoothing.py`
- [ ] Angles CSV: `data/processed/angles_exp001.csv`
- [ ] Figures: `reports/figures/fig_exp003_*.png`
- [ ] Experiment log entry

### Key Formulas
```
# Angle at joint B formed by A-B-C
def angle_2d(a, b, c):
    ba = a - b
    bc = c - b
    cosine = np.dot(ba, bc) / (np.linalg.norm(ba) * np.linalg.norm(bc))
    return np.degrees(np.arccos(np.clip(cosine, -1, 1)))
```

### Mini-Exercises
1. Convert keypoints CSV to long-format DataFrame
2. Calculate knee, hip, ankle angles for walking gait
3. Plot angle trajectories over gait cycle
4. Detect and interpolate missing keypoints
5. Compare raw vs smoothed angles

### Resources
- [Pandas Time Series](https://pandas.pydata.org/docs/user_guide/timeseries.html)
- [SciPy Signal Processing](https://docs.scipy.org/doc/scipy/reference/signal.html)
- [Gait Analysis Basics](https://www.physio-pedia.com/Gait_Analysis)

---

## Week 4: Comparing Methods and Research Foundations

### Learning Objectives
- Compare MoveNet, PoseNet, OpenPose, DeepLabCut
- Understand evaluation metrics for pose estimation
- Formulate research questions
- Design reproducible experiments

### Topics
1. **Comparing pose estimation approaches**
   - Architecture differences
   - Speed vs accuracy tradeoffs
   - Multi-person vs single-person
   - Pretrained vs custom training
   
2. **PoseNet, OpenPose, DeepLabCut**
   - PoseNet: ResNet/MobileNet backbones
   - OpenPose: Part Affinity Fields, multi-person
   - DeepLabCut: Transfer learning, custom keypoints
   
3. **Evaluation metrics**
   - PCK (Percentage of Correct Keypoints)
   - OKS (Object Keypoint Similarity)
   - AP (Average Precision) for keypoints
   - MPJPE (Mean Per Joint Position Error)
   - Temporal consistency metrics
   
4. **Research questions and limitations**
   - 2D vs 3D pose estimation
   - Camera angle effects
   - Clothing/occlusion robustness
   - Domain adaptation
   - Ethical considerations

### Expected Outputs
- [ ] Notebook: `notebooks/stage_4_method_comparison.ipynb`
- [ ] Updated: `docs/tools_comparison.md`
- [ ] Updated: `docs/research_notes.md`
- [ ] Experiment configs in `experiments/`
- [ ] Results in `reports/results.md`
- [ ] Decision records in `docs/decisions/`

### Evaluation Metrics to Implement
```python
# PCK @ threshold
def pck(pred, gt, threshold=0.2):
    # threshold as fraction of torso diameter
    dist = np.linalg.norm(pred - gt, axis=1)
    return np.mean(dist < threshold * torso_diameter)

# OKS (similar to COCO)
def oks(pred, gt, sigmas, area):
    # See COCO keypoint evaluation
    pass
```

### Mini-Exercises
1. Run PoseNet on sample video, compare with MoveNet
2. Try OpenPose (if GPU available) or document limitations
3. Design a simple evaluation protocol
4. Write 2-3 research questions for further investigation
5. Create decision record for model selection

### Resources
- [PoseNet GitHub](https://github.com/tensorflow/tfjs-models/tree/master/posenet)
- [OpenPose Paper](https://arxiv.org/abs/1812.08008)
- [DeepLabCut Paper](https://www.nature.com/articles/s41593-019-0458-0)
- [COCO Keypoint Evaluation](https://cocodataset.org/#keypoints-eval)

---

## Stage 4+: Sagittal-Plane Gait Analysis (Next Major Stage)

**This is a research requirement, not an optional extension.**

Our current frontal-view video is poorly suited for sagittal-plane flexion/extension measurements (hip/knee/ankle flexion primarily occurs in the sagittal plane). The next major stage **requires a suitable sagittal-plane walking video**.

### Prerequisites (Must Complete Before Stage 4)
1. **Select and validate sagittal-plane walking video** — Side view of single person walking, full body visible, minimal occlusion
2. **Camera view assessment** — Verify camera is perpendicular to walking direction
3. **Frame sampling & pose estimation** — Apply Stage 2 pipeline to new video
4. **Hip, knee, ankle keypoint trajectories** — Extract all three joint angles
5. **Temporal angle plots** — Stance/swing phase detection, gait cycle segmentation
6. **Occlusion handling** — Temporal smoothing, Kalman filtering, confidence-weighted interpolation
6. **Measurement reliability evaluation** — Confidence bands, outlier detection, ground-truth comparison if available

### Video Selection Criteria
- Sagittal (side) view, camera perpendicular to walking direction
- Single person, full body visible
- Multiple gait cycles (≥5 complete cycles)
- Good lighting, minimal background clutter
- Public domain / CC0 / permissive license (for redistribution in repo)
- Recommended sources: OpenSim, KIT Motion, CMU MoCap, or self-recorded

### Why Sagittal Plane?
- Hip/knee/ankle flexion/extension primarily occurs in sagittal plane
- Frontal view captures abduction/adduction poorly
- Standard gait analysis literature uses sagittal-plane kinematics
- Enables stance/swing phase detection from knee angle trajectory

### Beyond Stage 4: Further Extensions
- **3D Pose Estimation**: VideoPose3D, MediaPipe Pose 3D
- **Temporal Models**: TCN, LSTM for smoothing
- **Action Recognition**: Classify gait, activities
- **Multi-camera**: Triangulation for 3D
- **Domain Adaptation**: Sim-to-real, synthetic data
- **Real-time Optimization**: ONNX, TensorRT, TFLite