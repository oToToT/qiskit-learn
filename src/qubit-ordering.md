# Basis States, Bitstrings, And Qubit Order

This chapter exists because many beginners lose hours to bit ordering.

## The rule to remember

Qiskit uses little-endian ordering for basis states.

That means qubit `0` is the least significant bit in a computational-basis label.

For example, the label `|10>` means:

- qubit `1` is `1`
- qubit `0` is `0`

This feels backwards at first. You still need to internalize it.

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

All of these depend on qubit order:

- reading statevectors
- interpreting counts
- preparing a target basis state
- writing arithmetic circuits
- solving QCoder problems with explicit target strings

When a circuit looks almost right, ordering is one of the first things to check.

## State labels and measurement strings

The same bit-ordering convention shows up in measurement output.

If you do:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(2)
qc.x(0)
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=16).result()
print(result[0].data.meas.get_counts())
```

the counts show `"01"`, because qubit `1` is the left bit and qubit `0` is the right bit.

## A debugging habit that pays off

Whenever you target a basis state, write down both:

- which qubits you are changing
- which basis label you expect to see

For example, if you want `|101>`, say it out loud:

- qubit `2` should be `1`
- qubit `1` should be `0`
- qubit `0` should be `1`

That kind of explicitness prevents a large class of mistakes.

## A small table worth memorizing

For two qubits:

- `|00>` means `q1=0, q0=0`
- `|01>` means `q1=0, q0=1`
- `|10>` means `q1=1, q0=0`
- `|11>` means `q1=1, q0=1`

If this table feels natural, later arithmetic and QFT chapters become much easier.

## Checkpoint Exercises

1. Prepare `|10>` on two qubits.
2. Prepare `|101>` on three qubits.
3. Explain why `qc.x(0)` on two qubits gives `|01>`.
4. Create a small table for all 2-qubit basis states and the qubit values they represent.

## Try These On QCoder

- [QPC001 A4, Generate State (|10> + |11>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC001/problems/A4)
- [QPC003 A2, Generate State (|10> + |01>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC003/problems/A2)
- [QPC003 A3, Generate State (|100> + |010> + |001>)/sqrt(3)](https://www.qcoder.jp/en/contests/QPC003/problems/A3)
