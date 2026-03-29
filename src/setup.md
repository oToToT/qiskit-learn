# Setup

This project uses `mdBook` for the text and `uv` for Python environments and dependency management.

## Tools

Install:

- Python
- `uv`
- `mdbook`

The Python dependency metadata lives in `pyproject.toml` at the project root.

## Install the core Qiskit packages

From the project root:

```bash
uv sync
```

If you want to add packages later:

```bash
uv add qiskit
uv add qiskit-machine-learning
uv add jupyter
```

Run Python through `uv`:

```bash
uv run python
```

## Smoke test

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)
print(qc)
print(Statevector.from_instruction(qc))
```

If that works, your local environment is fine for the early chapters.

## Why this book uses exact simulation first

Beginners often jump to measurement too early.

For the first half of the book, you will mostly rely on:

- `Statevector` to inspect amplitudes exactly
- `StatevectorSampler` to sample measurement results without backend setup friction

That makes debugging much easier.
