# Two Qubits, Correlation, And Entanglement

Two qubits are where circuits start becoming interesting.

## The Bell state

This is the standard first entanglement example:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

state = Statevector.from_instruction(qc)
print(state)
print(qc)
```

The resulting state is:

\[
\frac{|00\rangle + |11\rangle}{\sqrt{2}}
\]

## What makes it special

If you measure both qubits many times, you should only see:

- `00`
- `11`

You should not see:

- `01`
- `10`

The qubits are correlated in a way that cannot be described as each qubit having its own independent classical value.

## The `cx` gate

Read `qc.cx(a, b)` as:

"flip qubit `b` if qubit `a` is `1`."

That simple rule is enough to build:

- Bell states
- controlled state preparation
- many small oracles
- Grover diffusion operators later

## A good beginner check

Compare these circuits:

```python
qc1 = QuantumCircuit(2)
qc1.h(0)
qc1.cx(0, 1)

qc2 = QuantumCircuit(2)
qc2.cx(0, 1)
qc2.h(0)
```

Same gates. Different order. Very different result.

Quantum programming is sensitive to order.

## DIY

Build and inspect:

1. \((|01\rangle + |10\rangle)/\sqrt{2}\)
2. a circuit that always ends at `|10>`
3. a circuit whose measurement results are perfectly correlated but biased toward one outcome after extra rotations
