# Architecture Decision Records (ADRs)

Lightweight decision log for the project. One file per decision.

## Format

```markdown
# ADR XXX: [Title]

## Status
[Proposed | Accepted | Superseded | Rejected]

## Date
YYYY-MM-DD

## Context
What situation led to this decision?

## Decision
What was decided?

## Consequences
- Positive:
- Negative:
- Neutral:

## Alternatives Considered
- Option A: ...
- Option B: ...

## Related
- ADR XXX
- Issue #XXX
```

---

## Index

| ID | Title | Status | Date |
|----|-------|--------|------|
| 001 | Project structure and conventions | Accepted | 2024-XX-XX |
| 002 | First pose estimation model: MoveNet | Accepted | 2024-XX-XX |
| 003 | Sample video source and license | Accepted | 2024-XX-XX |
| 004 | Keypoint data format (CSV schema) | Proposed | - |
| 005 | Evaluation metrics for no-ground-truth | Proposed | - |
| 006 | Temporal smoothing approach | Proposed | - |
| 007 | Custom keypoint definition for DeepLabCut | Proposed | - |

---

## ADR 001: Project Structure and Conventions

**Status:** Accepted
**Date:** 2024-10-18

**Context:** Need a clean, understandable project structure for an educational pose estimation project.

**Decision:** Use the following conventions:
- `data/raw` = immutable source data
- `data/interim` = regeneratable intermediate files
- `data/processed` = analysis-ready outputs
- `notebooks/` = exploratory work, one per stage
- `src/` = reusable modules with tests
- `experiments/` = YAML configs per experiment
- `reports/` = progress, log, results, figures
- `docs/` = learning materials, references, decisions
- `configs/` = parameter files for models/pipelines
- `scripts/` = standalone CLI utilities

**Consequences:**
- Clear separation of concerns
- Easy to navigate for educational purposes
- Git-friendly (small text files, large data in data/)
- Follows common data science conventions (Cookiecutter Data Science inspired)

---

## ADR 002: First Pose Estimation Model: MoveNet

**Status:** Accepted
**Date:** 2024-10-18

**Context:** Need a first model to establish the inference pipeline, keypoint extraction, and visualization workflow.

**Decision:** Start with **MoveNet Lightning** (TensorFlow Hub).

**Rationale:**
- Runs fast on CPU (~30-50 FPS) - no GPU required for learning
- Official TF Hub integration - easy installation
- Apache 2.0 license - can redistribute, commercial-friendly
- Lightweight (~3.2M params) - fast download, low memory
- Single-person - matches our sample video
- 17 COCO keypoints - standard format, good for learning

**Alternatives Considered:**
- **PoseNet:** Slower on CPU, Python ports less maintained
- **OpenPose:** Complex build, restrictive license, overkill for single person
- **MediaPipe Pose:** Good option, but MoveNet more established in TF ecosystem
- **YOLO-NAS-Pose / RTMPose:** Newer, but less educational material

**Consequences:**
- Pipeline built around regression-based model (no heatmaps)
- Confidence scores need calibration
- Single-person only - multi-person requires different model later
- TensorFlow dependency (but only for inference, not training yet)

---

## ADR 003: Sample Video Source and License

**Status:** Accepted
**Date:** 2024-10-18

**Context:** Need a single public video for all experiments. Must be legally redistributable in a public GitHub repo.

**Decision:** Use Pexels video "Man Walking on the Street" (ID: 9947048) under Pexels License.

**Rationale:**
- Pexels License permits free use, modification, redistribution
- No attribution required (but recommended)
- Single person, full body, side view - ideal for pose estimation
- HD available, ~10-20 seconds - manageable file size
- Static camera, clean background - reduces confounding factors

**Alternatives Considered:**
- **Pixabay:** Also permissive, similar content
- **Mixamo:** Requires Adobe account, license less clear for redistribution
- **Self-recorded:** Privacy concerns, effort, not reproducible
- **Public domain archives:** Often old, low quality, not ideal for pose

**Consequences:**
- Video committed to `data/raw/sample_walking.mp4` (if < 20MB)
- License documented in `docs/dataset_card.md`
- Attribution in `data/README.md`
- If video removed from Pexels, we have local copy

---

## How to Add a New ADR

```bash
# Copy template
cp docs/decisions/README.md docs/decisions/004_keypoint_format.md
# Edit with your decision
```

**Naming:** `NNN_short-title.md` (zero-padded, chronological)

**When to create an ADR:**
- Choosing a model, library, or algorithm
- Defining data formats or schemas
- Changing project structure
- Selecting evaluation methodology
- Any decision that would be hard to reverse or explain later

---

*Keep decisions lightweight - a few sentences each is fine.*