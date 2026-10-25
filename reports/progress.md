# Project Progress

## Current Stage
**Stage 2: MoveNet Inference & Analysis** — COMPLETED

## Completed Tasks
- [x] Project structure created
- [x] Configuration files created (requirements.txt, pyproject.toml, .gitignore, .editorconfig)
- [x] Documentation templates created
- [x] Experiment tracking templates created
- [x] Git repository initialized
- [x] Sample video identified and documented
- [x] Python dependencies installed in virtual environment
- [x] Sample video downloaded and verified
- [x] OpenCV video I/O verified
- [x] **Stage 1: Video processing with OpenCV**
- [x] **Stage 2: MoveNet inference & analysis**

## Pending Tasks
- [ ] Stage 3: Keypoint visualization and motion analysis
- [ ] Stage 4: PoseNet
- [ ] Stage 5: OpenPose
- [ ] Stage 6: DeepLabCut
- [ ] Stage 7: Evaluation and research questions

## Files Changed (Stage 1)
- `notebooks/stage_1_video_processing.ipynb` — executed notebook with all outputs
- `data/raw/sample_walking.mp4` — downloaded sample video (8.9 MB)
- `data/interim/extracted_frames/*.jpg` — 14 extracted frames
- `data/processed/resized_walking.mp4` — resized video (7.0 MB)
- `data/processed/annotated_walking.mp4` — annotated video v1 (18.4 MB)
- `data/processed/annotated_walking_v2.mp4` — annotated video v2 (16.7 MB)

## Files Changed (Stage 2)
- `notebooks/stage_2_pose_estimation.ipynb` — executed notebook with all outputs (94+ cells)
- `models/movenet_lightning.tflite` — MoveNet Lightning TFLite model (4.8 MB)
- `data/processed/letterboxed_keypoints.npy` — (if saved) keypoints in original coordinates
- `data/processed/letterboxed_model_keypoints.npy` — (if saved) raw model keypoints

## Latest Successful Checks
- Python 3.12.3 available
- Git 2.43.0 available
- GitHub CLI available (not authenticated)
- 20 GB disk space available
- FFmpeg installed and working
- Virtual environment created (`.venv/`)
- Dependencies installed: OpenCV 4.x, NumPy, Pandas, Matplotlib, Jupyter, PyYAML, tqdm, ai-edge-litert (TFLite)
- Jupyter kernel "Markerless Pose Estimation" registered
- Sample video opens correctly with OpenCV (1080×1920, 29.97 FPS, 405 frames)
- MoveNet Lightning TFLite model loaded successfully (input: 192×192×3 uint8, output: 1×1×17×3 float32)
- 41 frames sampled (every 10 frames), 17 keypoints × 41 frames extracted
- 18 valid knee-angle estimates (confidence ≥ 0.3, all landmarks visible)
- Letterbox preprocessing implemented and validated (scale=0.1, pad_left=42)

## Known Issues
- GitHub CLI not authenticated (cannot create remote repo automatically)
- Large generated video files in `data/processed/` are not gitignored (consider adding `data/processed/*.mp4` to .gitignore)
- 13/697 keypoints (1.9%) mapped outside original image bounds after letterbox inverse transform
- Right knee confidence drops to 0.288 at frame 90 (near suspected occlusion)
- Camera movement detected in video (affects displacement measurements)

## Next Recommended Step
1. Begin Stage 3: Keypoint visualization and motion analysis
   - Refine knee-angle time series with smoothing/interpolation
   - Calculate hip and ankle angles
   - Create temporal angle plots with confidence bands
   - Investigate occlusion handling strategies
   - Document Stage 3 experimental protocol
   - Create `experiments/exp_002_knee_angle_analysis.yaml`
2. Select and validate sagittal-plane walking video for Stage 4 gait analysis