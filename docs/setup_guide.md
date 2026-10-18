# Setup Guide

Step-by-step environment setup for the markerless pose estimation learning project.

---

## Prerequisites (System Level)

### Ubuntu / Debian

```bash
# Update package list
sudo apt update

# Install system dependencies
sudo apt install -y \
    python3 python3-venv python3-dev \
    git ffmpeg \
    libglib2.0-0 libsm6 libxext6 libxrender-dev libgl1-mesa-glx \
    build-essential cmake pkg-config

# Verify installations
python3 --version   # Should be 3.12+
git --version
ffmpeg -version
```

### FFmpeg Note

FFmpeg is required for:
- Reading various video formats in OpenCV
- Writing annotated output videos
- Frame extraction at scale

**If `ffmpeg` not found:** `sudo apt install ffmpeg`

---

## 1. Create Virtual Environment

```bash
# Navigate to project root
cd /path/to/markerless-pose-estimation-lab

# Create virtual environment
python3 -m venv .venv

# Activate (Linux/macOS)
source .venv/bin/activate

# Verify activation
which python   # Should point to .venv/bin/python
```

**Deactivate when done:** `deactivate`

---

## 2. Upgrade Pip and Install Dependencies

```bash
# Upgrade pip
pip install --upgrade pip setuptools wheel

# Install lightweight dependencies (requirements.txt)
pip install -r requirements.txt

# Verify key packages
python -c "
import cv2; print(f'OpenCV: {cv2.__version__}')
import numpy; print(f'NumPy: {numpy.__version__}')
import pandas; print(f'Pandas: {pandas.__version__}')
import matplotlib; print(f'Matplotlib: {matplotlib.__version__}')
import yaml; print(f'PyYAML: {yaml.__version__}')
import tqdm; print(f'tqdm: {tqdm.__version__}')
"
```

**Expected output (versions may vary):**
```
OpenCV: 4.8.x
NumPy: 1.26.x
Pandas: 2.1.x
Matplotlib: 3.8.x
PyYAML: 6.0.x
tqdm: 4.66.x
```

---

## 3. Register Jupyter Kernel

```bash
# Install ipykernel if not already
pip install ipykernel

# Register kernel
python -m ipykernel install --user --name=markerless-pose --display-name="Markerless Pose Estimation"

# Verify
jupyter kernelspec list | grep markerless
```

**Usage:**
- Start Jupyter: `jupyter lab` or `jupyter notebook`
- Select kernel: "Markerless Pose Estimation" when creating notebooks

---

## 4. Verify OpenCV Video I/O

```bash
# Test video reading (after downloading sample video)
python -c "
import cv2
cap = cv2.VideoCapture('data/raw/sample_walking.mp4')
if cap.isOpened():
    print('✓ Video opened successfully')
    print(f'  Frames: {int(cap.get(cv2.CAP_PROP_FRAME_COUNT))}')
    print(f'  FPS: {cap.get(cv2.CAP_PROP_FPS):.2f}')
    print(f'  Resolution: {int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))}x{int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))}')
    ret, frame = cap.read()
    print(f'  First frame: {frame.shape if ret else \"FAILED\"}')
else:
    print('✗ Failed to open video')
    print('  Check: file exists, ffmpeg installed, codec supported')
cap.release()
"
```

**If video fails to open:**
1. Confirm `data/raw/sample_walking.mp4` exists
2. Confirm `ffmpeg` is installed: `ffmpeg -version`
3. Try re-encoding: `ffmpeg -i data/raw/sample_walking.mp4 -c:v libx264 -preset fast data/raw/sample_walking_fixed.mp4`
4. Check OpenCV build: `python -c "import cv2; print(cv2.getBuildInformation())" | grep -i ffmpeg`

---

## 5. Download Sample Video

Follow instructions in [docs/dataset_card.md](dataset_card.md):

1. Visit: https://www.pexels.com/video/man-walking-on-the-street-9947048/
2. Click "Free Download" → Select HD (1920×1080)
3. Save as: `data/raw/sample_walking.mp4`
4. Verify with Step 4 commands

---

## 6. Optional: Development Tools

```bash
# Code formatting and linting
pip install black ruff mypy

# Testing
pip install pytest pytest-cov

# Pre-commit hooks (optional)
pip install pre-commit
pre-commit install
```

**Run checks:**
```bash
# Format
black src/ tests/ scripts/ notebooks/

# Lint
ruff check src/ tests/ scripts/

# Type check
mypy src/

# Tests
pytest
```

---

## 7. GPU Support (Optional, for Later Stages)

**Not required for initial stages (MoveNet Lightning runs well on CPU).**

### TensorFlow GPU (for MoveNet/DeepLabCut)
```bash
# CUDA 12.x, cuDNN 8.x required
pip install "tensorflow[and-cuda]"  # TF 2.15+
# Or specific version matching your CUDA
```

### PyTorch GPU (for OpenPose/DeepLabCut 2.3+)
```bash
# See https://pytorch.org/get-started/locally/
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

**Verify GPU:**
```bash
python -c "
import tensorflow as tf
print('TF GPU:', tf.config.list_physical_devices('GPU'))
import torch
print('Torch CUDA:', torch.cuda.is_available())
"
```

---

## 8. Directory Structure Verification

```bash
# From project root
tree -L 3 -I '.venv|__pycache__|.git'
```

**Expected:**
```
markerless-pose-estimation-lab/
├── README.md
├── LICENSE
├── .gitignore
├── .editorconfig
├── requirements.txt
├── pyproject.toml
├── data/
│   ├── README.md
│   ├── raw/
│   │   ├── .gitkeep
│   │   └── sample_walking.mp4    ← After download
│   ├── interim/
│   │   └── .gitkeep
│   └── processed/
│       └── .gitkeep
├── notebooks/
│   └── README.md
├── src/
│   └── README.md
├── tests/
│   └── README.md
├── configs/
│   └── README.md
├── experiments/
│   ├── README.md
│   └── experiment_template.yaml
├── reports/
│   ├── README.md
│   ├── progress.md
│   ├── experiment_log.md
│   ├── results.md
│   └── figures/
│       └── .gitkeep
├── docs/
│   ├── README.md
│   ├── learning_roadmap.md
│   ├── concepts.md
│   ├── tools_comparison.md
│   ├── dataset_card.md
│   ├── setup_guide.md
│   ├── research_notes.md
│   └── decisions/
│       └── README.md
└── scripts/
    └── README.md
```

---

## 9. Troubleshooting

### OpenCV ImportError: libGL.so.1
```bash
sudo apt install libgl1-mesa-glx libglib2.0-0
```

### OpenCV VideoCapture returns False
- Install ffmpeg: `sudo apt install ffmpeg`
- Re-encode video: `ffmpeg -i input.mp4 -c:v libx264 output.mp4`
- Check codec support: `python -c "import cv2; print(cv2.getBuildInformation())" | grep -i codec`

### Jupyter Kernel Not Showing
```bash
# Re-register
python -m ipykernel install --user --name=markerless-pose --display-name="Markerless Pose Estimation" --force
jupyter kernelspec list
```

### Permission Denied on Scripts
```bash
chmod +x scripts/*.py
```

### Git Ignores Data Files
- Check `.gitignore` - large data files may be ignored
- For sample video (<20MB): `git add data/raw/sample_walking.mp4` should work
- For larger files: consider Git LFS or external storage

---

## 10. Next Steps After Setup

1. **Confirm environment works:**
   ```bash
   source .venv/bin/activate
   python -c "import cv2; print('OpenCV OK')"
   ```

2. **Open Jupyter Lab:**
   ```bash
   jupyter lab
   ```
   Create first notebook: `notebooks/stage_1_video_processing.ipynb`

3. **Begin Stage 1:** Follow [docs/learning_roadmap.md](learning_roadmap.md) Week 1

4. **Track progress:** Update `reports/progress.md` after each session

---

## Quick Reference Card

| Command | Purpose |
|---------|---------|
| `source .venv/bin/activate` | Activate environment |
| `deactivate` | Deactivate environment |
| `pip install -r requirements.txt` | Install deps |
| `jupyter lab` | Start notebook server |
| `python -m ipykernel install --user --name=markerless-pose` | Register kernel |
| `ffprobe data/raw/sample_walking.mp4` | Check video properties |
| `black src/` | Format code |
| `pytest` | Run tests |
| `git status` | Check repo status |

---

*Keep this guide updated as environment evolves.*