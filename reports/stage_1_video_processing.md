# Stage 1: Video Processing with OpenCV — Progress Report

**Date:** 2026-10-20  
**Status:** COMPLETED  
**Notebook:** `notebooks/stage_1_video_processing.ipynb`

---

## Purpose

Establish foundational video processing skills using OpenCV, including reading video files, extracting metadata, sampling frames, resizing, writing output videos, drawing annotations, and building reusable utilities. This stage prepares the groundwork for pose estimation pipelines in later stages.

---

## Concepts Covered (Verified from Notebook)

| # | Concept | Verified in Notebook |
|---|---------|---------------------|
| 1 | Opening videos with `cv2.VideoCapture` | ✅ Cells 1, 4, 5, 17 |
| 2 | Reading frames and checking `ret` success | ✅ Cells 2, 4, 9, 17 |
| 3 | Video metadata (FPS, frame count, width, height, duration) | ✅ Cells 21-22 |
| 4 | Seeking to specific frames via `cap.set(cv2.CAP_PROP_POS_FRAMES)` | ✅ Cells 5, 6 |
| 5 | Selecting frames by timestamp (seconds → frame index) | ✅ Cell 6 |
| 6 | Extracting and saving frames with `cv2.imwrite` | ✅ Cells 7-10 |
| 7 | Temporal frame sampling: nominal 1-second vs exact timestamp | ✅ Cells 9-10 |
| 8 | Resizing images; understanding `(height, width, channels)` vs `cv2.resize((width, height))` | ✅ Cells 11-13 |
| 9 | Writing processed videos with `cv2.VideoWriter` (mp4v codec) | ✅ Cells 15, 19, 23 |
| 10 | Drawing annotations: `circle`, `line`, `rectangle`, `putText` | ✅ Cells 18, 19 |
| 11 | Building reusable video-processing functions | ✅ Cells 21 (`get_video_info`), 23 (`process_video`) |
| 12 | Safe resource release: `cap.release()`, `writer.release()`, `try/finally` | ✅ Cells 4, 15, 19, 23 |
| 13 | Image arrays: `uint8`, BGR vs RGB, shape interpretation | ✅ Cells 2, 25, 26 |
| 14 | Cropping / Region of Interest via array slicing | ✅ Cell 27 |
| 15 | Preserving aspect ratio when resizing (min scale factor) | ✅ Cell 33 |
| 16 | Letterboxing: padding resized image to fixed target size | ✅ Cell 34 |
| 17 | Mapping coordinates between letterboxed, resized, and original images | ✅ Cell 35 |
| 18 | Visualizing a **simulated** keypoint on an image | ✅ Cell 37 |

---

## Exercises Implemented

| Exercise | Description | Output Artifact |
|----------|-------------|-----------------|
| 1.1 | Open video, read first frame, display | Inline matplotlib figure |
| 1.2 | Read sequential frames (0–4) | Console output |
| 1.3 | Seek to frame index 100, display | Inline matplotlib figure |
| 1.4 | Seek by timestamps (1s, 3s, 5s, 8s), display | 4 inline matplotlib figures |
| 1.5 | Extract frames at specific timestamps to disk | `data/interim/extracted_frames/frame_001s.jpg` … `frame_008s.jpg` |
| 1.6 | Extract frames at 1-second intervals (every ~30 frames) | `data/interim/extracted_frames/frame_0000.jpg` … `frame_0013.jpg` |
| 1.7 | Count total readable frames (405) vs reported (405) | Console output |
| 1.8 | Resize frame directly to 540×960 (distorted) | `data/processed/resized_walking.mp4` |
| 1.9 | Resize preserving aspect ratio (256×144) | Inline matplotlib figure |
| 1.10 | Letterbox to 256×256 with padding | Inline matplotlib figure |
| 1.11 | Map model-space coordinates → original image coordinates | Console output: (100, 180) → (330, 1350) |
| 1.12 | Draw simulated keypoint on original frame | Inline matplotlib figure |
| 1.13 | Create annotated video (full resolution) | `data/processed/annotated_walking.mp4` |
| 1.14 | Create annotated video via reusable `process_video()` | `data/processed/annotated_walking_v2.mp4` |

---

## Verifiable Results (from Notebook Outputs)

| Metric | Value |
|--------|-------|
| **Video path** | `../data/raw/sample_walking.mp4` |
| **FPS** | 29.97002997002997 |
| **Frame count** | 405 |
| **Resolution (W×H)** | 1080 × 1920 (portrait) |
| **Duration** | ~13.5 seconds |
| **Frames successfully read** | 405 (matches reported count) |
| **Frames extracted at 1s intervals** | 14 frames (indices 0–13) |
| **Resized video** | 540×960, 405 frames, ~7 MB |
| **Annotated video v1** | 1080×1920, 405 frames, ~18 MB |
| **Annotated video v2** | 1080×1920, 405 frames, ~17 MB |
| **Pixel dtype** | `uint8` (0–255) |
| **Color order** | BGR (OpenCV default); converted to RGB for matplotlib |

---

## Output Artifacts

| File | Size | Description |
|------|------|-------------|
| `data/raw/sample_walking.mp4` | 8.9 MB | Source video (Pexels, permissive license) |
| `data/interim/extracted_frames/` | ~14 JPGs | Frames at specific timestamps + 1s interval |
| `data/processed/resized_walking.mp4` | 7.0 MB | Direct resize to 540×960 (distorted) |
| `data/processed/annotated_walking.mp4` | 18.4 MB | Annotated with circle, line, rect, frame counter |
| `data/processed/annotated_walking_v2.mp4` | 16.7 MB | Same, via reusable `process_video()` function |

---

## Known Limitations & Notes

1. **Simulated keypoint only** — The keypoint drawn in Cell 37 (`x_real=330, y_real=1350`) is a **hardcoded coordinate pair** used to demonstrate coordinate mapping and visualization. It is **not** an output from any pose estimation model (MoveNet, PoseNet, etc.). No actual pose estimation has been performed yet.

2. **Aspect ratio distortion** — The direct resize in Cell 15 (540×960) distorts the original 1080×1920 frame. The letterboxing approach (Cells 33–34) correctly preserves aspect ratio.

3. **Portrait orientation** — The source video is 1080×1920 (portrait). All resize/letterbox logic handles this correctly.

4. **Frame count verification** — Manual loop count (405) matches `CAP_PROP_FRAME_COUNT` (405.0), confirming reliable sequential read.

5. **Reusable functions** — `get_video_info()` and `process_video()` encapsulate common patterns with proper error handling and resource cleanup.

6. **No pose estimation yet** — Stage 1 is purely video I/O and preprocessing. MoveNet inference begins in Stage 2.

---

## Next Steps (Stage 2 Preview)

1. Install TensorFlow and TensorFlow Hub in the virtual environment.
2. Load MoveNet Lightning from TF Hub (`https://tfhub.dev/google/movenet/singlepose/lightning/4`).
3. Implement letterbox preprocessing (already built in Stage 1) to feed 256×256 frames to MoveNet.
4. Run inference on sample video, extract 17 COCO keypoints per frame.
5. Save keypoints to structured CSV (`data/processed/keypoints_exp001.csv`).
6. Visualize predicted keypoints (not simulated) on frames/video.
7. Log experiment as `exp_001` in `reports/experiment_log.md`.

---

## Files Modified/Created in This Session

- `notebooks/stage_1_video_processing.ipynb` — Main working notebook (executed, with outputs)
- `data/raw/sample_walking.mp4` — Downloaded sample video
- `data/interim/extracted_frames/*.jpg` — Extracted frames
- `data/processed/*.mp4` — Processed output videos