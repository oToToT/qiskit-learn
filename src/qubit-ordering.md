# Basis States, Bitstrings, And Qubit Order

This chapter exists because many beginners lose hours to bit ordering.

## The rule to remember

Qiskit uses little-endian ordering for basis states.

That means qubit `0` is the least significant bit in the computational basis label.

For example, the basis label `|10>` means:

- qubit `1` is `1`
- qubit `0` is `0`

## A quick sanity check

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.x(1)
print(Statevector.from_instruction(qc))
```

This prepares `|10>`, not `|01>`.

## Why this matters

Every one of these depends on bit ordering:

- reading statevectors
- interpreting counts
- preparing a target basis state
- writing arithmetic circuits
- solving QCoder problems with explicit target strings

When a circuit looks "almost right," ordering is one of the first places to check.

## Debugging habit

When you target a basis state, write down both:

- the qubits you flip
- the basis label you expect to see

That removes a lot of silent confusion.

## Checkpoint Exercises

1. Prepare `|10>` on two qubits.
2. Prepare `|101>` on three qubits.
3. Explain why `qc.x(0)` on two qubits gives `|01>`.
4. Create a small table for all 2-qubit basis states and the qubit values they represent.

## QCoder Connections

- QPC001 A4, "Generate State (|10> + |11>)/sqrt(2)"
- QPC003 A2, "Generate State (|10> + |01>)/sqrt(2)"
- QPC003 A3, "Generate State (|100> + |010> + |001>)/sqrt(3)"
