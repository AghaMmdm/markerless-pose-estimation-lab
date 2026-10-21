# A Hands-on Study of Markerless Human Pose Estimation

## Project Description

This is an educational project for learning markerless human pose estimation through hands-on experimentation. The project follows a step-by-step approach, starting from basic video processing with OpenCV and progressing through MoveNet, PoseNet, OpenPose, and DeepLabCut.

## Learning Objectives

1. Understand video processing with OpenCV
2. Understand human pose estimation and keypoints
3. Run MoveNet on a sample video
4. Understand PoseNet and its implementation approach
5. Explore OpenPose
6. Understand DeepLabCut and custom keypoint training
7. Extract keypoints and save them as structured data
8. Calculate simple joint angles
9. Visualize motion over time
10. Learn how to evaluate pose estimation methods
11. Develop an understanding of research questions and reproducible experiments

## Technologies Used

- **Python 3.12+**
- **OpenCV** - Video processing and computer vision
- **NumPy / Pandas** - Numerical computing and data manipulation
- **Matplotlib** - Visualization
- **Jupyter** - Interactive notebooks
- **MoveNet** (TensorFlow.js / TensorFlow) - Lightweight pose estimation
- **PoseNet** - Classic pose estimation model
- **OpenPose** - Multi-person pose estimation
- **DeepLabCut** - Custom keypoint training

## Project Roadmap

| Stage | Topic | Status |
|-------|-------|--------|
| 1 | Video processing with OpenCV | **COMPLETED** |
| 2 | MoveNet | NOT STARTED |
| 3 | Keypoint visualization and motion analysis | NOT STARTED |
| 4 | PoseNet | NOT STARTED |
| 5 | OpenPose | NOT STARTED |
| 6 | DeepLabCut | NOT STARTED |
| 7 | Evaluation and research questions | NOT STARTED |

## Repository Structure

```
markerless-pose-estimation-lab/
│
├── README.md
├── LICENSE
├── .gitignore
├── .editorconfig
├── requirements.txt
├── pyproject.toml
│
├── data/
│   ├── README.md
│   ├── raw/
│   │   └── .gitkeep
│   ├── interim/
│   │   └── .gitkeep
│   └── processed/
│       └── .gitkeep
│
├── notebooks/
│   └── README.md
│
├── src/
│   └── README.md
│
├── tests/
│   └── README.md
│
├── configs/
│   └── README.md
│
├── experiments/
│   ├── README.md
│   └── experiment_template.yaml
│
├── reports/
│   ├── README.md
│   ├── progress.md
│   ├── experiment_log.md
│   ├── results.md
│   └── figures/
│       └── .gitkeep
│
├── docs/
│   ├── learning_roadmap.md
│   ├── concepts.md
│   ├── tools_comparison.md
│   ├── dataset_card.md
│   ├── setup_guide.md
│   ├── research_notes.md
│   └── decisions/
│       └── README.md
│
└── scripts/
    └── README.md
```

## Installation Instructions

### Prerequisites

- Ubuntu Linux (or compatible)
- Python 3.12+
- Git
- FFmpeg (for video processing)

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/markerless-pose-estimation-lab.git
   cd markerless-pose-estimation-lab
   ```

2. **Create a virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install lightweight dependencies:**
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. **Register as Jupyter kernel:**
   ```bash
   python -m ipykernel install --user --name=markerless-pose --display-name="Markerless Pose Estimation"
   ```

5. **Verify OpenCV works:**
   ```bash
   python -c "import cv2; print(cv2.__version__)"
   ```

## Sample Video Preparation

The project uses a single short sample video of a person walking. See [data/README.md](data/README.md) and [docs/dataset_card.md](docs/dataset_card.md) for source, license, and download instructions.

If redistribution is permitted, the video will be at:
```
data/raw/sample_walking.mp4
```

Otherwise, follow the download instructions in the dataset card.

## Running Experiments

Each experimental stage will have its own notebook or script in `notebooks/` or `scripts/`. Experiment configurations are stored in `configs/` and results in `reports/`.

Use the experiment template:
```bash
cp experiments/experiment_template.yaml experiments/exp_001_movenet.yaml
```

## Current Project Status

**Stage 1 (Video Processing with OpenCV) completed.** The notebook `notebooks/stage_1_video_processing.ipynb` covers video I/O, frame extraction, resizing, letterboxing, annotation, and coordinate transformations. A sample video (`data/raw/sample_walking.mp4`) has been downloaded and processed. See `reports/stage_1_video_processing.md` for details.

Stages 2–7 are planned.

## Official Documentation Links

- [OpenCV Documentation](https://docs.opencv.org/)
- [MoveNet (TensorFlow Hub)](https://tfhub.dev/google/movenet/singlepose/lightning/4)
- [PoseNet](https://github.com/tensorflow/tfjs-models/tree/master/posenet)
- [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose)
- [DeepLabCut](https://www.deeplabcut.org/)
- [NumPy](https://numpy.org/doc/)
- [Pandas](https://pandas.pydata.org/docs/)
- [Matplotlib](https://matplotlib.org/stable/contents.html)

## Disclaimer

⚠️ **Important:** Pose estimation output from computer vision models is **not** automatically a medically validated measurement. Results are approximations suitable for educational and research purposes only. Clinical or medical applications require validated equipment and professional oversight.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.