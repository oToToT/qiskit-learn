# Reflections and Amplitude Amplification

Reflections are the second ingredient of Grover's algorithm. A reflection flips the sign of a state while leaving orthogonal states unchanged—the quantum equivalent of "reflecting" in a mirror.

## What Is a Reflection?

A reflection operator \\(R\\) about a state \\(|\\psi\\rangle\\):

\\[R = I - 2|\\psi\\rangle\\langle\\psi|\\]

Properties:

- \\(R|\\psi\\rangle = -|\\psi\\rangle\\) (the target state flips sign)
- \\(R|\\phi\\rangle = |\\phi\\rangle\\) for all \\(|\\phi\\rangle \\perp |\\psi\\rangle\\)
- \\(R^2 = I\\) (reflections are their own inverse)

## A Simple Reflection: Z Gate

On one qubit, \\(Z = I - 2|1\\rangle\\langle 1|\\):

- \\(Z|1\\rangle = -|1\\rangle\\)
- \\(Z|0\\rangle = |0\\rangle\\)

This is a reflection about the \\(|0\\rangle\\) state.

## Reflection About Any State

To reflect about \\(|\\psi\\rangle\\):

1. Prepare \\(|\\psi\\rangle\\) from \\(|0\\rangle\\)
1. Apply \\(Z\\) on ancilla
1. Unprepare \\(|\\psi\\rangle\\)

\\[R\_\\psi = U(I - 2|0\\rangle\\langle 0|)U^\\dagger = I - 2|\\psi\\rangle\\langle\\psi|\\]

where \\(U|0\\rangle = |\\psi\\rangle\\).

## Example: Reflect About \\(|+\\rangle\\)

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Reflection about |+⟩
# Since H|0⟩ = |+⟩, we have: H Z H = X = I - 2|+⟩⟨+|
qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)
qc.h(0)

# Verify: this equals X
from qiskit.quantum_info import Operator
print("X:", Operator(QuantumCircuit(1, lambda qc: qc.x(0))))
print("H Z H:", Operator(qc))
print("Equal:", Operator(QuantumCircuit(1, lambda qc: qc.x(0))) == Operator(qc))
```

## Reflection About Superposition States

Reflection about \\(|+\\rangle\\) flips \\(|-\\rangle\\) but leaves \\(|+\\rangle\\) unchanged.

Try it:

```python
qc = QuantumCircuit(1)
qc.x(0)  # |−⟩
qc.h(0)
qc.z(0)
qc.h(0)
print("Flip |−⟩:", Statevector(qc))
```

## The Diffusion Operator

Grover's diffusion operator reflects about the uniform superposition:

\\[D = H^{\\otimes n}(I - 2|0\\rangle\\langle 0|^{\\otimes n})H^{\\otimes n} = I - 2\\sum_x|x\\rangle\\langle x|/N\\]

```python
def diffusion(n):
    qc = QuantumCircuit(n)
    
    # Reflect about |0...0⟩
    qc.x(range(n))
    qc.h(n-1)
    qc.mcx(list(range(n-1)), n-1)  # Multi-controlled Z
    qc.h(n-1)
    qc.x(range(n))
    
    # Equivalently: H X X ... X H
    return qc
```

## Amplitude Amplification

Combining oracle + diffusion:

1. Oracle marks target states (flips sign)
1. Diffusion reflects about uniform superposition
1. Target amplitudes grow

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def amplitude_amplification_1iter(n, target):
    qc = QuantumCircuit(n)
    
    # Start in uniform superposition
    qc.h(range(n))
    
    # Oracle: mark target state
    for i, bit in enumerate(target):
        if bit == '1':
            qc.x(i)
    qc.h(n-1)
    qc.mcx(list(range(n-1)), n-1)
    qc.h(n-1)
    for i, bit in enumerate(target):
        if bit == '1':
            qc.x(i)
    
    # Diffusion
    qc.h(range(n))
    qc.x(range(n))
    qc.h(n-1)
    qc.mcx(list(range(n-1)), n-1)
    qc.h(n-1)
    qc.x(range(n))
    qc.h(range(n))
    
    return qc

# Test on 2 qubits, target |11⟩
qc = amplitude_amplification_1iter(2, '11')
state = Statevector(qc)
print("After 1 iteration:", state)
print("P(|11⟩):", state.probabilities()[3])
```

## Checkpoint Exercises

### Exercise 1

Verify that Z is a reflection about \\(|0\\rangle\\).

### Exercise 2

Build a reflection about \\(|11\\rangle\\) on 2 qubits.

### Exercise 3

Explain why \\(HZH = X\\) is a reflection about \\(|+\\rangle\\).

### Exercise 4

Design a reflection about \\((|00\\rangle + |11\\rangle)/\\sqrt{2}\\).

### Exercise 5

Show that two reflections in succession amplify the marked amplitude.

## Summary

Key concepts:

- Reflections flip sign of target state
- \\(Z\\) reflects about \\(|0\\rangle\\)
- \\(X\\) reflects about \\(|+\\rangle\\)
- Diffusion reflects about uniform superposition
- Oracle + Diffusion = Amplitude Amplification

Reflections are the mechanism behind Grover's search speedup.

______________________________________________________________________

## Try These On QCoder

- [QPC003 B4: Reflection Operator I](https://www.qcoder.jp/en/contests/QPC003/problems/B4)
- [QPC003 B5: Reflection Operator II](https://www.qcoder.jp/en/contests/QPC003/problems/B5)
- [QPC003 B7: Reflection Operator III](https://www.qcoder.jp/en/contests/QPC003/problems/B7)

______________________________________________________________________

*Next: [Grover Search](./grover.md)*
