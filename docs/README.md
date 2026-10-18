# Documentation Directory

Project documentation, learning materials, and research notes.

## Files

- `learning_roadmap.md` - Week-by-week learning plan
- `concepts.md` - Glossary of key terms
- `tools_comparison.md` - Comparison of pose estimation tools
- `dataset_card.md` - Dataset/video provenance and license
- `setup_guide.md` - Environment setup instructions
- `research_notes.md` - Ongoing research observations
- `decisions/` - Architecture and design decision records

## Decision Records

Use `decisions/` for Architecture Decision Records (ADRs):

```
decisions/
├── 001-use-movenet-first.md
├── 002-keypoint-format.md
└── 003-evaluation-metrics.md
```

Format:
```markdown
# ADR 001: Use MoveNet as First Model

## Status
Accepted

## Context
Need a lightweight, CPU-friendly model to start.

## Decision
Use MoveNet Lightning (TensorFlow Hub).

## Consequences
- Fast iteration on CPU
- Limited to 17 COCO keypoints
- Single-person only
```