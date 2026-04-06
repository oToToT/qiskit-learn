# Phase Kickback: Encoding in Phase

Phase kickback is one of the most elegant tricks in quantum computing. It lets you transfer information from control qubits to phases—enabling many quantum algorithms.

## The Core Idea

When a controlled operation acts on an **eigenstate** of the target operation, the control qubit picks up the eigenvalue as a phase.

In simpler terms: a controlled operation can move phase information from the target to the control.

## The Simplest Example

The \\(|-\\rangle\\) state is an eigenstate of X:

\\[X|-\\rangle = -|-\\rangle\\]

This means:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Prepare |-⟩ on qubit 1
qc = QuantumCircuit(2)
qc.x(1)      # |1⟩
qc.h(1)      # H|1⟩ = |−⟩

# Apply CNOT with |−⟩ as target
qc.cx(0, 1)

print(Statevector(qc))
```

The \\(|1\\rangle\\) component on qubit 0 picks up a minus sign!

## What Happened?

Before:
\\[|0\\rangle \\otimes |-\\rangle + |1\\rangle \\otimes |-\\rangle = (|0\\rangle + |1\\rangle) \\otimes |-\\rangle\\]

After CNOT:
\\[|0\\rangle \\otimes |-\\rangle + |1\\rangle \\otimes X|-\\rangle = |0\\rangle \\otimes |-\\rangle - |1\\rangle \\otimes |-\\rangle = (|0\\rangle - |1\\rangle) \\otimes |-\\rangle\\]

The control qubit now has the phase!

## The Key Identity

Remember from the basis change chapter:

\\[HZH = X\\]

This means:

- \\(|+\\rangle\\) is the \\(+1\\) eigenstate of \\(Z\\) (eigenvalue \\(+1\\))
- \\(|-\\rangle\\) is the \\(-1\\) eigenstate of \\(Z\\) (eigenvalue \\(-1\\))

And:

- \\(|0\\rangle\\) is the \\(+1\\) eigenstate of \\(Z\\) (eigenvalue \\(+1\\))
- \\(|1\\rangle\\) is the \\(-1\\) eigenstate of \\(Z\\) (eigenvalue \\(-1\\))

## Phase Kickback with Z

Using \\(Z|1\\rangle = -|1\\rangle\\):

```python
# Kickback with Z
qc = QuantumCircuit(2)
qc.h(0)      # Put control in superposition
qc.x(1)      # Prepare |1⟩ (eigenstate of Z with eigenvalue -1)
qc.cz(0, 1) # Controlled-Z

print(Statevector(qc))
```

The \\(|1\\rangle\\) branch on qubit 0 picks up a minus sign.

## Why Is This Useful?

Phase kickback lets you:

1. Encode classical information in phases
1. Transfer phase information between qubits
1. Create controlled operations from simpler ones
1. Build oracles for Grover's algorithm

## A Concrete Application: Phase Oracle

A phase oracle marks a target state \\(|w\\rangle\\) by flipping its phase:

\\[O_w|x\\rangle = \\begin{cases} -|x\\rangle & \\text{if } x = w \\ |x\\rangle & \\text{otherwise} \\end{cases}\\]

For \\(|w\\rangle = |11\\rangle\\):

```python
# Phase oracle for |11⟩
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)  # Put both in superposition
qc.cz(0, 1)  # Mark |11⟩
qc.h(0)
qc.h(1)

print(Statevector(qc))
```

Try this and verify that \\(|11\\rangle\\) gets the minus sign.

## The Phase Kickback Pattern

The standard phase kickback pattern:

```python
def phase_kickback(n_qubits, eigenstate_prep, controlled_op):
    """Generic phase kickback circuit."""
    qc = QuantumCircuit(n_qubits)
    
    # 1. Put control qubits in superposition
    for i in range(n_qubits - 1):
        qc.h(i)
    
    # 2. Prepare eigenstate on ancilla
    eigenstate_prep(qc)
    
    # 3. Apply controlled operation
    controlled_op(qc)
    
    return qc
```

## Basis Changes Reveal Phase

After phase kickback, you can't see the phase in computational basis measurement. Change to X basis:

```python
# Create phase, then measure in X basis
qc = QuantumCircuit(2)
qc.h(0)
qc.x(1)
qc.cz(0, 1)

# Measure in X basis
qc.h(0)  # Change to X basis
qc.measure_all()

from qiskit.primitives import StatevectorSampler
sampler = StatevectorSampler()
result = sampler.run([qc], shots=100).result()
print(result[0].data.meas.get_counts())
```

The phase becomes a bit flip in the X basis.

## Checkpoint Exercises

### Exercise 1

Show that \\(HZH = X\\) using phase kickback logic.

### Exercise 2

Create a circuit where qubit 0 picks up phase \\(\\pi/2\\) (not \\(\\pi\\)).

### Exercise 3

Build a phase oracle that marks \\(|00\\rangle\\).

### Exercise 4

Explain why \\(|+\\rangle\\) and \\(|-\\rangle\\) measure the same in Z basis but differently in X basis.

### Exercise 5

Design a circuit that transfers phase from qubit A to qubit B.

## Summary

Key concepts:

- Phase kickback: controlled operations on eigenstates create phase on controls
- \\(|-\\rangle\\) is the \\(-1\\) eigenstate of \\(X\\)
- \\(|1\\rangle\\) is the \\(-1\\) eigenstate of \\(Z\\)
- Phase kickback enables oracles and many algorithms

Phase kickback is fundamental to understanding Grover's algorithm and quantum phase estimation.

______________________________________________________________________

## Try These On QCoder

- [QPC003 B2: Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1: Convert Bit-Flip into Phase-Flip II](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)
- [QPC005 B1: Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)

______________________________________________________________________

*Next: [Reversible Logic and Oracles](./reversible-logic.md)*
