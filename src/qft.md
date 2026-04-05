# Quantum Fourier Transform

The Quantum Fourier Transform (QFT) is a quantum version of the discrete Fourier transform. It's a key subroutine in many quantum algorithms, including Shor's factoring algorithm.

## What QFT Does

QFT transforms between two representations of quantum data:

1. **Computational basis**: states labeled by binary strings
1. **Fourier basis**: states labeled by phases

Mathematically:
\\[|x\\rangle \\xrightarrow{\\text{QFT}} \\frac{1}{\\sqrt{N}}\\sum\_{k=0}^{N-1} e^{2\\pi i xk/N}|k\\rangle\\]

## The 2-Qubit QFT

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from math import pi

def qft_2():
    qc = QuantumCircuit(2)
    qc.h(1)
    qc.cp(pi/2, 0, 1)  # Controlled phase
    qc.h(0)
    qc.swap(0, 1)
    return qc

qc = qft_2()
print(qc)
print(Statevector.from_instruction(qc))
```

## The Pattern

The n-qubit QFT follows a pattern:

1. Apply Hadamard to most significant qubit
1. Apply controlled phases with decreasing angles
1. Repeat on remaining qubits
1. Swap at the end (to fix qubit ordering)

```python
def qft(n):
    qc = QuantumCircuit(n)
    for i in range(n-1, -1, -1):
        qc.h(i)
        for j in range(i):
            qc.cp(pi/(2**(i-j)), j, i)
    qc.swap(*range(n//2))  # Swap pairs
    return qc
```

## Why Controlled Phases?

The Fourier basis states are distinguished by phase patterns:

```
QFT|00⟩ = (|00⟩ + |01⟩ + |10⟩ + |11⟩)/2        (all same phase)
QFT|01⟩ = (|00⟩ - |01⟩ + |10⟩ - |11⟩)/2       (alternating)
QFT|10⟩ = (|00⟩ + i|01⟩ - |10⟩ - i|11⟩)/2     (phase pattern)
QFT|11⟩ = (|00⟩ - i|01⟩ - |10⟩ + i|11⟩)/2     (different pattern)
```

The controlled phases build these phase patterns.

## QFT and Qubit Order

QFT is usually defined with big-endian ordering (qubit n-1 is most significant).
Qiskit uses little-endian.
The final SWAP fixes this.

```python
# Without SWAP: output appears reversed
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)
qc.cp(pi/2, 0, 1)

print("Without swap:", Statevector.from_instruction(qc))
```

## Inverse QFT

QFT is unitary, so it has an inverse (conjugate transpose):

```python
qc = QuantumCircuit(2)
qc.x(0)  # Start with |01⟩

# Apply QFT then inverse QFT
qc.compose(qft_2(), inplace=True)
qc.compose(qft_2().inverse(), inplace=True)

print("After QFT† QFT:", Statevector.from_instruction(qc))
```

If implemented correctly, you get back to the original state.

## Fourier Basis

The Fourier basis states are eigenstates of phase operations:

\\[|j\\rangle\_{\\text{Fourier}} \\xrightarrow{\\text{Phase}} e^{2\\pi i j/N}|j\\rangle\_{\\text{Fourier}}\\]

This is why QFT is useful for phase estimation.

## Checkpoint Exercises

### Exercise 1

Implement the 3-qubit QFT by hand.

### Exercise 2

Apply QFT to each basis state and describe the phase patterns.

### Exercise 3

Verify that QFT followed by inverse QFT returns to the original state.

### Exercise 4

Remove the SWAP gates and explain what changes.

### Exercise 5

Create a circuit that adds phase \\(e^{2\\pi i/4}\\) to the \\(|1\\rangle\\) state.

## Summary

Key concepts:

- QFT transforms between computational and Fourier bases
- Built from Hadamards and controlled phases
- SWAP gates fix qubit ordering
- Inverse QFT is QFT with negated phases
- Foundation for phase estimation and Shor's algorithm

QFT is the quantum version of a fundamental mathematical operation. Understanding it opens the door to many quantum algorithms.

______________________________________________________________________

## Try These On QCoder

- [QPC002 B4: Quantum Fourier Transform](https://www.qcoder.jp/en/contests/QPC002/problems/B4)
- [QPC005 B1: Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)
- [QPC005 B2: Cyclic Shift in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B2)

______________________________________________________________________

*Next: [Arithmetic Circuits and Modular Thinking](./arithmetic.md)*
