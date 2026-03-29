# Quantum Fourier Transform

QFT is the next big milestone after Grover because it teaches you to think in phase structure rather than only marked states.

## What QFT does

At a high level, the Quantum Fourier Transform changes basis so that periodic or phase-encoded structure becomes easier to read.

That sentence can sound abstract, so keep one simpler description nearby:

QFT is a structured basis change built from Hadamards and controlled phase rotations.

## The 2-qubit QFT by hand

A small QFT circuit is the best place to start.

```python
from qiskit import QuantumCircuit

def qft_2():
    qc = QuantumCircuit(2)
    qc.h(1)
    qc.cp(3.141592653589793 / 2, 0, 1)
    qc.h(0)
    qc.swap(0, 1)
    return qc

print(qft_2())
```

This already shows the structure:

- apply a Hadamard
- add controlled phase rotations with decreasing angles
- repeat on the next qubit
- optionally reverse qubit order with swaps

## Why controlled phases appear

In the Fourier basis, basis states are distinguished by phase patterns. Controlled phase rotations are the local mechanism that builds those patterns one qubit at a time.

You do not need the full matrix formula to understand the circuit behavior:

- Hadamards create local superposition
- controlled phases correlate branches by angle
- swaps restore the output qubit order you usually want

## QFT and qubit order are tightly linked

Many QFT confusions are not about Fourier analysis. They are about:

- little-endian ordering
- where the swaps belong
- whether the inverse QFT is being used

That is one reason the earlier qubit-order chapter matters so much.

## QFT followed by inverse QFT

The most important first verification is that QFT is invertible and that your implementation is correct.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.x(0)
qc.compose(qft_2(), inplace=True)
qc.compose(qft_2().inverse(), inplace=True)

print(Statevector.from_instruction(qc))
```

If the implementation is correct, you recover the original input state.

## Looking at phases directly

To get a more useful feel for QFT, inspect the transformed state of a basis vector:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.x(0)
qc.compose(qft_2(), inplace=True)
print(Statevector.from_instruction(qc))
```

You will see that the output is not a single basis state. It is a superposition whose amplitudes differ by phase.

That is the key shift in thinking:

- Grover highlights marked branches
- QFT highlights structured phase relationships

## When the final swaps matter

Some derivations and library routines omit the trailing swaps and treat the reversed order as acceptable.

That is mathematically fine if you track the ordering consistently. It is a common beginner mistake if you do not.

So the practical rule is:

- keep the swaps while learning
- remove them only when you are very clear about the resulting bit order

## Where QFT shows up later

QFT is a building block for:

- phase estimation
- period finding
- arithmetic in the Fourier basis
- shift and modular-action circuits

Even if you do not implement all of those immediately, QFT is worth understanding as a reusable basis-change pattern.

## Checkpoint Exercises

1. Write a 2-qubit QFT circuit by hand.
2. Remove the final swaps and describe exactly what changes.
3. Apply QFT and inverse QFT in sequence and verify you recover the input state.
4. Inspect the QFT of each 2-qubit basis state and describe the phase pattern you see.

## Try These On QCoder

- [QPC002 B4, Quantum Fourier Transform](https://www.qcoder.jp/en/contests/QPC002/problems/B4)
- [QPC005 B1, Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)
- [QPC005 B2, Cyclic Shift in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B2)
