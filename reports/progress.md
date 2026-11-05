# Project Progress

## Current Stage
**Stage 5: MoveNet vs DeepLabCut Comparison** — COMPLETED

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
- [x] **Stage 3: Quantitative gait analysis**
- [x] **Stage 4: DeepLabCut fundamentals & pretrained inference**
- [x] **Stage 5: MoveNet vs DeepLabCut comparison**

## Pending Tasks
- [ ] Stage 6: Research experiments: alternative anatomical landmarks
- [ ] Stage 7: Evaluation and research questions

## Files Changed (Stage 1)
- `notebooks/stage_1_video_processing.ipynb` — executed notebook with all outputs
- `data/raw/sample_walking.mp4` — downloaded sample video (8.9 MB)
- `data/interim/extracted_frames/*.jpg` — 17 extracted frames
- `data/processed/resized_walking.mp4` — resized video (7.0 MB)
- `data/processed/annotated_walking.mp4` — annotated video v1 (18.4 MB)
- `data/processed/annotated_walking_v2.mp4` — annotated video v2 (16.7 MB)

## Files Changed (Stage 2)
- `notebooks/stage_2_pose_estimation.ipynb` — executed notebook with all outputs (94+ cells)
- `models/movenet_lightning.tflite` — MoveNet Lightning TFLite model (4.8 MB)
- `reports/stage_2_pose_estimation.md` — detailed experimental report

## Files Changed (Stage 3)
- `notebooks/stage_3_gait_analysis.ipynb` — executed notebook (22 cells)
- `data/raw/sagittal_walking.mp4` — sagittal walking video (32 MB)

## Files Changed (Stage 4)
- `notebooks/stage_4_deeplabcut_fundamentals.ipynb` — executed notebook (39 cells)
- `data/raw/sagittal_walking.mp4` — sagittal walking video (32 MB)
- `data/processed/deeplabcut/` — DeepLabCut output directory

## Files Changed (Stage 5)
- `reports/stage_5_movenet_dlc_comparison.md` — detailed comparison report
- `data/processed/comparison/movenet_dlc_knee_angles.csv` — paired knee angles (82 frames)
- `data/processed/comparison/agreement_metrics.csv` — MAE, RMSE, Pearson correlation
- `data/processed/comparison/bias_analysis.csv` — mean bias, difference std
- `data/processed/comparison/detection_coverage.csv` — detection counts per model

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
- DeepLabCut 3.0.2 loaded successfully (PyTorch 2.14.1+cu126, CUDA, RTX 4060)
- MoveNet: 41 frames sampled (every 10 frames), 17 keypoints × 41 = 697 predictions
- DeepLabCut: 406 frames processed (every frame), 17 keypoints per frame
- Comparison: 82 matched frames, right knee MAE 9.34°, correlation 0.732; left knee MAE 20.94°, correlation −0.073

## Known Issues
- GitHub CLI not authenticated (cannot create remote repo automatically)
- Large generated video files in `data/processed/` are not gitignored (consider adding `data/processed/*.mp4` to .gitignore)
- `ai-edge-litert` not in requirements.txt — dependency not documented
- Sagittal video license undocumented — source and redistribution rights not recorded
- Stage 3 and Stage 4 notebook modifications uncommitted
- Stage 3 cell 20 empty and unexecuted
- Stage 3 no report file exists yet

## Next Recommended Step
1. Commit uncommitted notebook changes (`stage_2_pose_estimation.ipynb`, `stage_3_gait_analysis.ipynb`, `stage_4_deeplabcut_fundamentals.ipynb`)
2. Add `ai-edge-litert` to `requirements.txt`
3. Document sagittal video source/license in `docs/dataset_card.md`
4. Write `reports/stage_3_gait_analysis.md`
5. Investigate alternative anatomical landmarks (Stage 6) — landmark configuration not yet finalized with supervisor
6. Do not start manual annotation or DeepLabCut custom training yet