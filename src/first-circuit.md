# Your First Circuit

The first Qiskit object to get comfortable with is `QuantumCircuit`.

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.h(0)
print(qc)
```

This says:

- create a circuit with one qubit
- apply a Hadamard gate to qubit `0`

The important part is not the syntax. The important part is the meaning.

Starting from `|0>`, the `H` gate makes an equal superposition:

\[
\frac{|0\rangle + |1\rangle}{\sqrt{2}}
\]

## Looking at the state directly

Before measuring, it helps to inspect the exact quantum state.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)

state = Statevector.from_instruction(qc)
print(state)
```

This is one of the best beginner tools in Qiskit. It lets you inspect amplitudes without introducing sampling noise.

## The beginner mental model

Think of a circuit as a recipe that transforms a state step by step.

At this stage:

- `QuantumCircuit` is the recipe
- gates are transformations
- `Statevector.from_instruction(...)` lets you inspect the result exactly

## DIY

Write three tiny circuits and inspect each statevector:

1. apply `x(0)`
2. apply `z(0)`
3. apply `x(0)` and then `h(0)`

Do not just run them. Predict the result first.
