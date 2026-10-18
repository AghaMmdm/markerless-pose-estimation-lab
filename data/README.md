# Data Directory

This directory contains all data used in the project, organized by processing stage.

## Structure

```
data/
├── raw/        # Original, immutable data (downloaded videos, datasets)
├── interim/    # Intermediate processed data (extracted frames, temporary files)
└── processed/  # Final processed data ready for analysis (keypoints, angles, etc.)
```

## Sample Video

The project uses a single public domain / CC-licensed video of a person walking.

**Source:** [Mixamo / Adobe Stock - Walking Animation](https://www.mixamo.com/) or similar public domain source
**License:** To be verified - see [docs/dataset_card.md](../docs/dataset_card.md)
**File:** `data/raw/sample_walking.mp4` (if redistribution permitted)

## Usage Guidelines

- **Never modify files in `raw/`** - treat as immutable source data
- **`interim/`** - safe to delete and regenerate
- **`processed/`** - version-controlled outputs (CSV, JSON) may be committed if small
- Large generated files (>10MB) should be gitignored or stored externally

## Data Provenance

All data sources, licenses, and download dates are documented in [docs/dataset_card.md](../docs/dataset_card.md).