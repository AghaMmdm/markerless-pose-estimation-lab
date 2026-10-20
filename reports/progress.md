# Project Progress

## Current Stage
**Stage 1: Video Processing with OpenCV** — COMPLETED

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

## Pending Tasks
- [ ] Stage 2: MoveNet inference
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

## Latest Successful Checks
- Python 3.12.3 available
- Git 2.43.0 available
- GitHub CLI available (not authenticated)
- 20 GB disk space available
- FFmpeg installed and working
- Virtual environment created (`.venv/`)
- Dependencies installed: OpenCV 4.x, NumPy, Pandas, Matplotlib, Jupyter, PyYAML, tqdm
- Jupyter kernel "Markerless Pose Estimation" registered
- Sample video opens correctly with OpenCV (1080×1920, 29.97 FPS, 405 frames)

## Known Issues
- GitHub CLI not authenticated (cannot create remote repo automatically)
- Large generated video files in `data/processed/` are not gitignored (consider adding `data/processed/*.mp4` to .gitignore if not meant for version control)

## Next Recommended Step
1. Begin Stage 2: MoveNet inference
   - Install TensorFlow: `pip install tensorflow tensorflow-hub`
   - Load MoveNet Lightning from TF Hub
   - Use letterbox preprocessing (from Stage 1) to prepare 256×256 input
   - Run inference on sample video
   - Save keypoints to CSV
   - Visualize predictions
   - Create experiment config: `cp experiments/experiment_template.yaml experiments/exp_001_movenet.yaml`
   - Log results in `reports/experiment_log.md` and `reports/results.md`