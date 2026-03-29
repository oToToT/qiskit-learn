# Setup

This project uses `mdBook` for the text and `uv` for Python environments and dependencies.

## Tools you need

Install:

- Python 3.11 or newer
- `uv`
- `mdbook`

The Python dependency metadata lives in `pyproject.toml`.

## Install the project dependencies

From the project root, run:

```bash
uv sync
```

That creates a local environment and installs the pinned dependencies for this book.

If you want to add more packages later, use `uv add`, not `pip`:

```bash
uv add qiskit
uv add qiskit-machine-learning
uv add jupyter
```

Run Python through `uv` so you are using the project environment:

```bash
uv run python
```

## Smoke test

This is the smallest useful Qiskit test:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)

print(qc)
print(Statevector.from_instruction(qc))
```

If that prints a one-qubit circuit and a state with equal amplitudes on `|0>` and `|1>`, your environment is ready for the early chapters.

## Build the book locally

To build the static site:

```bash
mdbook build
```

To serve it locally with live reload:

```bash
mdbook serve --open
```

## Why this book uses exact simulation first

Beginners often measure too early and then wonder why nothing makes sense.

For the first half of the book, your main tools are:

- `Statevector` for exact amplitudes
- `StatevectorSampler` for shot-based sampling without backend setup

That order is deliberate:

1. inspect the state
2. predict the distribution
3. sample the distribution

If you reverse that order, debugging becomes much harder.

## Optional extras

A few later sections mention additional packages and workflows.

- For notebooks, add `jupyter`
- For quantum machine learning examples, add `qiskit-machine-learning`
- For experimentation, keep a small `examples/` or notebook folder outside `src/` so your book pages stay clean
