# Source Code Directory

Reusable Python modules and utility functions developed during the project.

## Structure

```
src/
├── utils/          # Common utilities (video I/O, visualization, etc.)
├── models/         # Model wrappers (MoveNet, PoseNet, etc.)
├── analysis/       # Analysis functions (angles, metrics, statistics)
└── pipeline/       # Experiment pipeline components
```

## Guidelines

- Write clean, documented functions with type hints
- Keep modules small and focused
- Add docstrings following NumPy style
- Write tests in `tests/` for non-trivial functions
- Avoid hardcoded paths - use config files