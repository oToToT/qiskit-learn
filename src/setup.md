# Setup

Let's get your environment ready. This chapter walks you through installing everything you need to work through this book.

## What You're Installing

You need three things:

| Tool | Purpose | Version |
|------|---------|---------|
| **Python** | Programming language | 3.13+ |
| **uv** | Package manager and environment tool | Latest |
| **mdbook** | Book build tool | Latest |
| **Qiskit** | Quantum computing framework | Latest |

## Step 1: Install Python

Check if you have Python installed:

```bash
python --version
```

If you see `Python 3.13` or higher, you're good. Otherwise, install Python from [python.org](https://www.python.org/downloads/) or use your system package manager.

## Step 2: Install uv

`uv` is a fast Python package manager. It replaces `pip` for this project.

### On macOS/Linux:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### On Windows:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### With pip (alternative):

```bash
pip install uv
```

Verify the installation:

```bash
uv --version
```

## Step 3: Install mdbook

mdbook builds this tutorial into a nice-looking website.

### On macOS (with Homebrew):

```bash
brew install mdbook
```

### On Linux (with cargo):

```bash
cargo install mdbook
```

### From binary (any OS):

Download from the [releases page](https://github.com/rust-lang/mdBook/releases) and add to your PATH.

Verify the installation:

```bash
mdbook --version
```

## Step 4: Install Project Dependencies

This project uses `uv` to manage Python dependencies. The dependencies are defined in `pyproject.toml`.

```bash
cd /path/to/qiskit-learn
uv sync
```

This creates a virtual environment and installs:

- `qiskit` — the main quantum computing framework
- Other dependencies as needed

**Important:** Use `uv sync`, not `pip install`. The project is configured to use uv.

## Step 5: Run Python with uv

Always run Python through uv to use the project environment:

```bash
# Run a single script
uv run python my_script.py

# Open an interactive shell
uv run python
```

## Adding Packages

If you need to add packages, use `uv add`:

```bash
# Add a new package
uv add qiskit-machine-learning

# Add a development package
uv add --dev pytest
```

Never use `pip install` directly—it bypasses the project's dependency management.

## The Smoke Test

Let's verify everything works. Create a file called `test_setup.py`:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create a simple circuit: one qubit, one Hadamard gate
qc = QuantumCircuit(1)
qc.h(0)

# Print the circuit diagram
print("Circuit:")
print(qc)

# Print the quantum state
print("\nStatevector:")
state = Statevector(qc)
print(state)
```

Run it:

```bash
uv run python test_setup.py
```

You should see:

```
Circuit:
    ┌───┐
 q: ┤ H ├
    └───┘

Statevector:
Statevector([0.70710678+0.j, 0.70710678+0.j],
            dims=(2,))
```

If you see something like this, your environment is ready!

## Building the Book

To build the book into a static website:

```bash
mdbook build
```

To serve it locally with live reload:

```bash
mdbook serve --open
```

The `--open` flag automatically opens the book in your browser.

## Project Structure

Here's what the project looks like:

```
qiskit-learn/
├── book.toml          # mdbook configuration
├── pyproject.toml     # Python dependencies
├── uv.lock            # Locked dependency versions
├── book/              # Built book output
│   └── index.html
└── src/               # Book source files
    ├── SUMMARY.md     # Table of contents
    ├── introduction.md
    ├── setup.md
    └── ...
```

## Why Statevector-First?

You might wonder why this book emphasizes `Statevector` over measurement.

Here's why:

1. **Exact answers** — Statevector gives you the exact quantum state, not statistical samples
1. **Faster debugging** — You can see exactly what's happening, not just averages
1. **Phase visibility** — Relative phase is invisible in counts but visible in statevectors
1. **Foundation for understanding** — Measurement statistics follow from the statevector

The workflow we'll use:

1. Write circuit
1. Inspect `Statevector()`
1. Understand the state
1. Then: measure and sample

This order will save you hours of confusion.

## Optional: Jupyter Notebooks

If you prefer Jupyter notebooks for experimentation:

```bash
uv add jupyter
uv run jupyter lab
```

This lets you run code interactively and keep notes alongside your circuits.

> **Note:** The official Qiskit documentation includes many Jupyter notebooks at [docs.quantum.ibm.com](https://docs.quantum.ibm.com/). After completing this tutorial, those notebooks become much more accessible.

## Optional: Qiskit Machine Learning

If you want to explore quantum machine learning in the later chapters:

```bash
uv add qiskit-machine-learning
```

This is optional—the core chapters don't need it.

## Practice Utilities

Here's a helper module you can save as `practice.py` for interactive verification:

```python
"""Practice utilities for quantum computing exercises."""
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector, Operator
from qiskit.visualization import plot_bloch_multivector
from qiskit.primitives import StatevectorSampler

def verify_state(qc: QuantumCircuit, expected_state: Statevector, verbose: bool = True) -> bool:
    """Verify that a circuit produces the expected state."""
    actual = Statevector(qc)
    match = actual.equiv(expected_state)
    if verbose:
        print(f"Circuit:\n{qc}")
        print(f"Expected: {expected_state}")
        print(f"Actual:   {actual}")
        print(f"Match: {match}")
    return match

def visualize_and_print(qc: QuantumCircuit):
    """Display Bloch sphere and print state for a circuit."""
    state = Statevector(qc)
    print(f"Circuit:\n{qc}\nState: {state}")
    display(plot_bloch_multivector(state))

def compare_gates(gate1: QuantumCircuit, gate2: QuantumCircuit, names: tuple = ("Gate1", "Gate2")):
    """Compare two gate sequences to check equivalence."""
    op1, op2 = Operator(gate1), Operator(gate2)
    equal = op1.equiv(op2)
    print(f"{names[0]}:")
    print(gate1)
    print(f"{names[1]}:")
    print(gate2)
    print(f"Equivalent: {equal}")
    return equal

def sample_and_print(qc: QuantumCircuit, shots: int = 1000) -> dict:
    """Measure a circuit and print the sample results."""
    qc_copy = qc.copy()
    qc_copy.measure_all()
    sampler = StatevectorSampler()
    result = sampler.run([qc_copy], shots=shots).result()
    counts = result[0].data.meas.get_counts()
    print(f"Circuit:\n{qc}\nMeasurement ({shots} shots): {counts}")
    return counts

# Example usage:
# from practice import verify_state, visualize_and_print, compare_gates
# from qiskit import QuantumCircuit
# from qiskit.quantum_info import Statevector

# verify_state(QuantumCircuit.from_qasm_str("OPENQASM 2.0; include 'qelib1.inc'; qreg q[1]; h q[0];"), 
#               Statevector([1/2**0.5, 1/2**0.5]))
```

Save this file in your project directory and import it as needed during exercises.

> **Tip:** For interactive exploration, run `uv run python` to get a REPL where you can import these utilities and experiment with circuits.

## Troubleshooting

### "uv: command not found"

Close and reopen your terminal after installing uv. If it still doesn't work, check that uv is in your PATH.

### "Python version not supported"

This book requires Python 3.13 or newer. Update Python and try again.

### "qiskit import fails"

Make sure you're using `uv run python`, not just `python`. This ensures you're using the project environment with qiskit installed.

### "mdbook build fails"

Check that mdbook is installed and in your PATH:

```bash
mdbook --version
```

## Summary

At the end of this setup, you should have:

- [ ] Python 3.13+
- [ ] uv installed and working
- [ ] mdbook installed and working
- [ ] Project dependencies installed (`uv sync`)
- [ ] Smoke test passing

If any of these aren't working, go back and check that step. Don't skip ahead until the smoke test passes—everything else depends on a working environment.

______________________________________________________________________

*Next: [Your First Circuit](./first-circuit.md)*
