# Setup

This project uses `mdBook` for the tutorial and `uv` for Python work.

## Tooling

You need:

- `python`
- `uv`
- `mdbook`

Inside this folder, the Python metadata lives in `pyproject.toml` at the project root.

## Install Qiskit with `uv`

From the project root:

```bash
uv add qiskit
```

If you want notebooks later:

```bash
uv add jupyter
```

Then sync the environment:

```bash
uv sync
```

Run Python commands through `uv` so you use the project environment:

```bash
uv run python
```

## Smoke test

Once `qiskit` is installed, this should work:

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.x(0)
print(qc)
```

## How we will simulate circuits

For most early chapters, we do not need a heavy simulator backend. We can learn a lot with:

- `Statevector` for exact amplitudes
- `StatevectorSampler` for repeated measurement samples

That keeps the focus on circuits instead of infrastructure.

## One habit worth keeping

Create a new file for each experiment. Do not keep one giant scratch file that mixes five unrelated ideas.

A simple structure works well:

```text
examples/
  01-first-circuit.py
  02-measurement.py
  03-bell-state.py
```
