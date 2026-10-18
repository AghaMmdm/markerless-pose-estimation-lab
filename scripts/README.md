# Scripts Directory

Standalone executable scripts for data preparation, batch processing, and utilities.

## Structure

```
scripts/
├── download_sample_video.py    # Download sample video (if permitted)
├── extract_frames.py           # Extract frames from video
├── run_inference.py           # Batch inference on video
├── compute_angles.py          # Calculate joint angles from keypoints
└── visualize_results.py       # Generate plots and videos
```

## Guidelines

- Scripts should be runnable from project root: `python scripts/name.py`
- Use `argparse` for command-line arguments
- Load configuration from `configs/`
- Write outputs to `data/` or `reports/`
- Include `--help` documentation