# Dataset / Sample Video Card

Following the [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) format, adapted for a single educational video.

---

## 1. Video Description

**Title:** Man Walking on the Street (Pexels ID: 9947048)
**Description:** A single adult male walking on a city street, captured from a side/profile angle showing full body. Suitable for pose estimation: clear silhouette, minimal occlusion, consistent background.
**Category:** Human locomotion / gait analysis
**Intended use:** Educational benchmark for markerless pose estimation (MoveNet, PoseNet, OpenPose, DeepLabCut)

---

## 2. Source

**Primary URL:** https://www.pexels.com/video/man-walking-on-the-street-9947048/
**Platform:** Pexels (https://www.pexels.com)
**Contributor/Photographer:** To be confirmed on video page (Pexels typically credits the creator)
**Date accessed:** [FILL IN AFTER DOWNLOAD]
**Download date:** [FILL IN AFTER DOWNLOAD]

**Alternative sources (if primary unavailable):**
- Pexels search: "man walking alone full body" → filter: "Free to use", "HD", "Vertical/Horizontal"
- Pixabay: https://pixabay.com/videos/search/walking%20alone/ → filter: "Free for commercial use", "No attribution required"

---

## 3. License

**License:** [Pexels License](https://www.pexels.com/license/)
**Summary:**
- ✅ Free to use for commercial and non-commercial purposes
- ✅ No attribution required (but appreciated)
- ✅ Modification allowed
- ✅ Redistribution allowed (including in this GitHub repository)
- ❌ Cannot sell unaltered copies as stock content
- ❌ Cannot imply endorsement by photographer or Pexels
- ❌ Cannot use identifiable people in offensive/illegal contexts

**Redistribution verdict:** **PERMITTED** - The video can be committed to `data/raw/sample_walking.mp4` in this repository.

**Attribution (recommended):**
> Video by [Photographer Name] from Pexels (https://www.pexels.com/video/man-walking-on-the-street-9947048/)

---

## 4. Download Instructions

### Option A: Manual Download (Recommended)

1. Open https://www.pexels.com/video/man-walking-on-the-street-9947048/ in browser
2. Click **"Free Download"** button
3. Select **"HD 1920x1080"** or **"SD 1280x720"** (prefer HD for quality, SD for speed)
4. Save as `data/raw/sample_walking.mp4`

### Option B: Command Line (if direct URL known)

```bash
# Note: Pexels does not provide stable direct MP4 URLs.
# Manual download via browser is recommended.
# If you have a direct URL, use:
wget -O data/raw/sample_walking.mp4 "DIRECT_MP4_URL"
```

### Option C: Alternative Video (Pixabay)

If Pexels video is unavailable:
1. Go to https://pixabay.com/videos/search/walking%20alone/
2. Filter: "Free for commercial use" ✓, "No attribution required" ✓
3. Select a video with: single person, full body, side view, 10-30 seconds
4. Download HD/MP4
5. Save as `data/raw/sample_walking.mp4`

---

## 5. Video Specifications (Expected)

| Property | Value | Notes |
|----------|-------|-------|
| **Duration** | ~10-20 seconds | Typical Pexels free clip length |
| **Resolution** | 1920×1080 (HD) or 1280×720 (SD) | Download HD, can downsample |
| **Frame Rate** | 25 or 30 FPS | Check with `ffprobe` after download |
| **Codec** | H.264 (AVC) | Standard MP4 |
| **Color Space** | YUV 4:2:0 | Standard |
| **File Size** | ~5-20 MB (HD, 10-20s) | Manageable for Git |
| **Audio** | Usually none or ambient | Not needed for pose estimation |

**Actual values (fill after download):**
- Duration: [FILL IN]
- Resolution: [FILL IN]
- Frame Rate: [FILL IN]
- Codec: [FILL IN]
- File Size: [FILL IN] bytes
- Original Filename: [FILL IN]

---

## 6. Verification Commands

After download, verify the video:

```bash
# Check file exists and size
ls -lh data/raw/sample_walking.mp4

# Probe video properties
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,r_frame_rate,codec_name,duration \
  -of csv=p=0 data/raw/sample_walking.mp4

# Quick OpenCV test
python -c "
import cv2
cap = cv2.VideoCapture('data/raw/sample_walking.mp4')
print(f'Frames: {int(cap.get(cv2.CAP_PROP_FRAME_COUNT))}')
print(f'FPS: {cap.get(cv2.CAP_PROP_FPS)}')
print(f'Width: {int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))}')
print(f'Height: {int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))}')
ret, frame = cap.read()
print(f'First frame shape: {frame.shape if ret else \"FAILED\"}')
cap.release()
"
```

---

## 7. Intended Educational Use

| Stage | Use Case |
|-------|----------|
| Stage 1 | Video I/O, frame extraction, FPS/resolution analysis |
| Stage 2 | MoveNet inference baseline |
| Stage 3 | Keypoint visualization, angle calculation, gait cycle analysis |
| Stage 4 | PoseNet comparison |
| Stage 5 | OpenPose multi-person test (single person in video) |
| Stage 6 | DeepLabCut custom keypoint labeling (e.g., foot contact points) |

**Why this video?**
- Single person (simplifies Stages 1-4)
- Full body visible (all 17 COCO keypoints observable)
- Side/profile view (good for sagittal plane angles: knee, hip, ankle)
- Walking gait (periodic motion, clear cycles)
- Static camera (no camera motion compensation needed)
- Clean background (minimal false positives)
- Permissive license (can share in public repo)

---

## 8. Known Limitations

| Limitation | Impact | Mitigation |
|------------|--------|------------|
| Single subject only | Cannot test multi-person until Stage 5 | Use OpenPose demo images for multi-person |
| No ground truth keypoints | Cannot compute quantitative accuracy (MPJPE, PCK) | Qualitative evaluation only; synthetic data for metrics |
| Single camera angle | 2D only, view-dependent angles | Acknowledge in analysis; don't claim 3D validity |
| Clothing may occlude joints | Knee/ankle confidence may vary | Use confidence thresholds; interpolate |
| Short duration (~15s) | Limited gait cycles (~10-15 steps) | Loop video for longer analysis if needed |
| Lighting/background fixed | No domain variation testing | Supplement with other videos later |
| Unknown camera intrinsics | Cannot metricize (mm) without calibration | Report in pixels; note limitation |

---

## 9. Ethical Considerations

- **Consent:** Pexels contributors upload voluntarily with model releases where required
- **Identifiability:** Face may be visible; blur if publishing annotated frames publicly
- **Bias:** Single demographic (adult male); not representative of population
- **Use case:** Educational/research only; not for surveillance, medical, or security applications

---

## 10. Maintenance

**Review date:** [FILL IN - 6 months from download]
**Action:** Verify license still valid; check for better/updated sample videos

---

## 11. Quick Start Checklist

- [ ] Video downloaded to `data/raw/sample_walking.mp4`
- [ ] `ffprobe` verification passed
- [ ] OpenCV test reads first frame successfully
- [ ] License confirmed (Pexels License)
- [ ] Attribution noted in `data/README.md`
- [ ] File committed to Git (if < 20 MB) or Git LFS / .gitignore if larger

---

*Update this card after actual download with real values.*