# Your First Circuit

The first Qiskit object you need is `QuantumCircuit`.

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.h(0)
print(qc)
```

This says:

- create a one-qubit circuit
- apply a Hadamard gate to qubit `0`

Starting from `|0>`, the `H` gate creates the state

$$
\frac{|0\rangle + |1\rangle}{\sqrt{2}}
$$

## Look at the state before measuring

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)

state = Statevector.from_instruction(qc)
print(state)
```

This is the most important beginner debugging move in Qiskit. Before asking what you will observe, ask what state you built.

## Recipes and states

Keep these roles separate:

- the circuit is the recipe
- the statevector is the current quantum state
- measurement turns the state into classical outcomes

If you mix those ideas together, every later chapter gets harder.

## Checkpoint Exercises

1. Prepare `|1>`.
2. Prepare the plus state.
3. Apply `x` then `h` and inspect the resulting state.
4. Build two different circuits that end in the same state.

## QCoder Connections

These are natural follow-ups once this chapter feels easy:

- QPC001 A1, "Generate State |1>"
- QPC001 A2, "Generate Plus state"
- QPC002 B1, "Generate State e^(i theta)|0>"
