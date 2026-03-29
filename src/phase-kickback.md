# Phase Kickback And Basis Changes

Phase kickback is the point where many circuits stop looking like gate puzzles and start looking algorithmic.

## The shortest useful explanation

If a controlled operation targets a state that is an eigenstate of the target operation, then the control path can pick up phase information.

For beginners, the practical meaning is simpler:

- you can encode information in phase
- later basis changes can reveal it

## The key identity again

\\[ H Z H = X \\]

This is the smallest example of a powerful pattern:

1. phase information exists in one basis
2. switch basis
3. it becomes bit-flip information

## The standard `|->` ancilla trick

The state

\\[ |-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} \\]

is special because `X|-\rangle = -|-\rangle`.

That means a controlled-`X` aimed at a `|->` ancilla can transfer a sign back to the control condition.

## A concrete example

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)

# Put the data qubit in superposition.
qc.h(0)

# Prepare ancilla in |->
qc.x(1)
qc.h(1)

# Controlled-X from data to ancilla
qc.cx(0, 1)

print(Statevector.from_instruction(qc))
```

If you inspect the final state carefully, the branch where the control qubit is `1` picks up a minus sign.

## Why basis changes matter so much

A phase can be hard to see if you stay in the computational basis. A Hadamard often makes the effect visible.

For example:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)
qc.h(0)
print(Statevector.from_instruction(qc))
```

This ends in `|1>`. The middle `z` looked like a phase operation, but after basis changes it behaves like a bit flip.

## Why QCoder likes this topic

Several QCoder problems are really asking whether you can move cleanly between:

- bit-flip language
- phase-flip language
- reflection language

If you can do that, Grover and Fourier-basis problems become much easier.

## Checkpoint Exercises

1. Show with Qiskit that `H Z H` and `X` act the same on `|0>` and `|1>`.
2. Prepare a minus-state ancilla and test a controlled bit-flip against a phase oracle.
3. Explain in one paragraph why equal measurement counts can still hide useful phase structure.
4. Build a circuit that converts a phase condition into a measurable bit condition.

## Try These On QCoder

- [QPC003 B2, Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1, Convert Bit-Flip into Phase-Flip II](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)
- [QPC005 B1, Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)
