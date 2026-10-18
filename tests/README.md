# Tests Directory

Unit tests for source code in `src/`.

## Structure

```
tests/
├── test_utils.py
├── test_models.py
├── test_analysis.py
└── test_pipeline.py
```

## Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src
```

## Guidelines

- Test pure functions and critical logic
- Use fixtures for common test data
- Mock external dependencies (models, network)
- Keep tests fast (<1s each)