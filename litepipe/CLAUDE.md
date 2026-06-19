# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Litepipe is a lightweight Python library for building data pipelines using Apache Beam-inspired syntax. It uses operator overloading (`|` for pipeline composition, `>>` for labeling transforms) to chain transforms together.

## Build & Test Commands

```bash
# Activate virtualenv
source env/bin/activate

# Run all tests
python -m pytest litepipe/tests/

# Run a single test file
python -m pytest litepipe/tests/test_pipeline.py

# Run a single test
python -m pytest litepipe/tests/test_pipeline.py::PipelineTest::test_single_transform

# Run tests with coverage
python -m coverage run -m pytest litepipe/tests/ && python -m coverage report

# Install package locally (editable)
pip install -e .
```

## Architecture

The library has three core classes in `litepipe/`:

- **`Transform`** (`transform.py`): Abstract base class. Subclasses implement `expand(input)` which must `yield` outputs. `__or__` (`|`) adds child transforms (creating a DAG). `__rshift__` (`>>`) sets a display label. `__call__` runs the transform on input, collecting outputs and propagating to children.

- **`Create`** (`transform.py`): A `Transform` subclass that serves as a pipeline source — wraps a static list of inputs.

- **`GroupBy`** (`transform.py`): Groups input by the first character (via `get_key()`), yielding `{"key": ..., "values": [...]}` dicts.

- **`Pipeline`** (`pipeline.py`): Entry point. `__or__` sets the start transform. `run()` executes the DAG and collects results. `stream()` runs in streaming mode. `graph` property serializes the transform tree.

**Key pattern**: Transforms form a tree via `children` list. A transform with multiple children produces branching pipelines. Execution flows depth-first through the tree using generators (`yield from`).

**Custom transforms**: Subclass `Transform` and implement `expand(self, input)` — must use `yield`, not `return`. See `litepipe/examples/transforms.py` for examples.

## Testing

Tests use `unittest` (not pytest fixtures). Example transforms used by tests live in `litepipe/examples/transforms.py`.
