# State Preparation Patterns

QCoder loves state-preparation tasks because they expose whether you understand gates as transformations rather than names.

## Pattern 1: basis states

To prepare `|101>`, flip the right qubits:

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(3)
qc.x(0)
qc.x(2)
```

## Pattern 2: equal superposition

To create an equal superposition over all `n`-qubit basis states:

```python
qc = QuantumCircuit(n)
for i in range(n):
    qc.h(i)
```

## Pattern 3: branch on one qubit, route with controls

Many hand-built two- and three-qubit states are easiest to construct by:

1. setting amplitudes on one qubit
2. using controlled gates to route amplitude to the correct basis states

## Pattern 4: fix the signs after the probabilities

Two states can share the same measurement distribution and still differ meaningfully:

$$
\frac{|00\rangle + |11\rangle}{\sqrt{2}}
\quad\text{and}\quad
\frac{|00\rangle - |11\rangle}{\sqrt{2}}
$$

That is when `z`, `cz`, and basis changes matter.

## Pattern 5: build symmetry deliberately

States like

$$
\frac{|100\rangle + |010\rangle + |001\rangle}{\sqrt{3}}
$$

are good training because they force you to think in amplitudes, not just bit flips.

## Checkpoint Exercises

1. Prepare the Bell state.
2. Prepare the sign-flipped Bell state.
3. Prepare the 3-qubit one-hot superposition above.
4. Prepare a uniform superposition on four qubits.

## QCoder Connections

- QPC001 A5, "Generate State (sqrt(3)/2)|100> + (1/2)|011>"
- QPC003 A3, "Generate State (|100> + |010> + |001>)/sqrt(3)"
- QPC003 A4 through A6, the one-hot superposition sequence
