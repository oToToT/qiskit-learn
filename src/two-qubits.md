# Controlled Gates, Correlation, And Entanglement

The `cx` gate is where many circuits stop feeling like independent one-qubit manipulations.

## Read `cx` correctly

```python
qc.cx(control, target)
```

means:

"flip the target if the control is `1`."

That sentence is enough to understand most beginner two-qubit circuits.

## The Bell state

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
print(Statevector.from_instruction(qc))
```

This prepares

$$
\frac{|00\rangle + |11\rangle}{\sqrt{2}}
$$

If you sample it, you only see `00` and `11`.

## Correlation versus entanglement

At a beginner level, the practical rule is:

- a correlated measurement pattern is easy to observe
- entanglement is the quantum structure underneath some of those patterns

You do not need full formal definitions yet. You do need to become fluent with Bell-state construction and inspection.

## Order matters

These are not the same:

```python
qc1.h(0); qc1.cx(0, 1)
qc2.cx(0, 1); qc2.h(0)
```

Quantum circuits are ordered transformations, not unordered gate bags.

## Checkpoint Exercises

1. Prepare `( |01> + |10> ) / sqrt(2)`.
2. Prepare `|10>` using exactly one `x` gate.
3. Build a circuit that creates a Bell state and then maps it to `( |00> - |11> ) / sqrt(2)`.
4. Compare `cx(0, 1)` and `cx(1, 0)` on the same input state.

## QCoder Connections

- QPC001 A4, "Generate State (|10> + |11>)/sqrt(2)"
- QPC002 B3, "SWAP Qubits"
- QPC003 A2, "Generate State (|10> + |01>)/sqrt(2)"
