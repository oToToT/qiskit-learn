# More Two-Qubit Gates: CZ, SWAP

Beyond CNOT, Qiskit provides several useful two-qubit gates. We'll cover the most important ones: CZ (Controlled-Z) and SWAP.

## The CZ Gate (Controlled-Z)

The CZ gate applies a Z operation when the control is \\(|1\\rangle\\):

\\[CZ|ij\\rangle = (-1)^{ij}|ij\\rangle\\]

| Input | Output |
|-------|--------|
| `\|00⟩` | `\|00⟩` |
| `\|01⟩` | `\|01⟩` |
| `\|10⟩` | `\|10⟩` |
| `\|11⟩` | `-\|11⟩` |

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# CZ on |11⟩ gives -|11⟩
qc = QuantumCircuit(2)
qc.x([0, 1])  # Prepare |11⟩
qc.cz(0, 1)

print("CZ|11⟩:", Statevector.from_instruction(qc))
```

Output: \\(-|11\\rangle\\)

### CZ Creates Phase Entanglement

On superposition states, CZ creates phase correlations:

```python
# Create superposition then apply CZ
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)
qc.cz(0, 1)

print("CZ on superposition:", Statevector.from_instruction(qc))
```

The \\(|11\\rangle\\) component picks up a minus sign.

### CZ vs CNOT

| Gate | Effect on `\|11⟩` | Effect on `\|10⟩` | Effect on `\|01⟩` |
|------|-------------------|-------------------|-------------------|
| CNOT | `\|10⟩` (flip) | `\|11⟩` (flip) | `\|01⟩` (no change) |
| CZ | `-\|11⟩` (phase) | `\|10⟩` (no change) | `\|01⟩` (no change) |

## The SWAP Gate

The SWAP gate exchanges the states of two qubits:

\\[SWAP|ij\\rangle = |ji\\rangle\\]

| Input | Output |
|-------|--------|
| `\|00⟩` | `\|00⟩` |
| `\|01⟩` | `\|10⟩` |
| `\|10⟩` | `\|01⟩` |
| `\|11⟩` | `\|11⟩` |

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# SWAP |01⟩ → |10⟩
qc = QuantumCircuit(2)
qc.x(1)  # |01⟩
qc.swap(0, 1)
print("SWAP|01⟩:", Statevector.from_instruction(qc))
```

## Implementing SWAP with CNOTs

SWAP can be decomposed into three CNOTs:

```python
def swap_via_cnot(qc, a, b):
    """Swap qubits a and b using three CNOTs."""
    qc.cx(a, b)
    qc.cx(b, a)
    qc.cx(a, b)

# Verify it's equivalent to SWAP
qc1 = QuantumCircuit(2)
qc1.swap(0, 1)

qc2 = QuantumCircuit(2)
swap_via_cnot(qc2, 0, 1)

from qiskit.quantum_info import Operator
print("SWAP == 3×CNOT:", Operator(qc1) == Operator(qc2))
```

## Other Two-Qubit Gates

### iSWAP

Performs a swap with an additional phase:

\\[iSWAP|01\\rangle = i|10\\rangle\\]
\\[iSWAP|10\\rangle = i|01\\rangle\\]

```python
qc = QuantumCircuit(2)
qc.iswap(0, 1)
```

### RZZ (Controlled-RZ)

A controlled rotation around the Z axis:

```python
from math import pi
qc = QuantumCircuit(2)
qc.rzz(pi/4, 0, 1)
```

### Controlled-H

Hadamard controlled on one qubit:

```python
qc = QuantumCircuit(2)
qc.ch(0, 1)  # Controlled-H
```

## Gate Decompositions

Many "native" gates are actually decomposed into simpler gates on real hardware:

```python
# CZ can be made from CNOT + H on target
qc = QuantumCircuit(2)
qc.h(1)  # H on target
qc.cx(0, 1)
qc.h(1)  # H on target

# Verify: CZ == H-CNOT-H
from qiskit.quantum_info import Operator
print("H-CNOT-H == CZ:", Operator(qc) == Operator(QuantumCircuit(2, circuit=qc).compose(QuantumCircuit(2))))
```

Actually, let's verify this correctly:

```python
cz = QuantumCircuit(2)
cz.cz(0, 1)

h_cnot_h = QuantumCircuit(2)
h_cnot_h.h(1)
h_cnot_h.cx(0, 1)
h_cnot_h.h(1)

print("CZ == H-CNOT-H:", Operator(cz) == Operator(h_cnot_h))
```

## Summary of Two-Qubit Gates

| Gate | Symbol | Effect | Code |
|------|--------|--------|------|
| CNOT | `■───⊕` | Flip target if control=1 | `cx(c, t)` |
| CZ | `■───■` | Phase flip if both=1 | `cz(c, t)` |
| SWAP | `×───×` | Exchange qubit states | `swap(a, b)` |
| iSWAP | `iSWAP` | Swap with phase | `iswap(a, b)` |

## Checkpoint Exercises

### Exercise 1

Create a circuit that swaps \\(|10\\rangle\\) to \\(|01\\rangle\\).

### Exercise 2

Show that CZ on \\(|++\\rangle\\) (both qubits in plus state) gives a phase difference.

### Exercise 3

Verify that SWAP = CNOT(0,1) CNOT(1,0) CNOT(0,1).

### Exercise 4

Use CZ to create a phase-entangled state starting from \\(|++\\rangle\\).

### Exercise 5

Create a circuit that applies CZ only when both qubits are in \\(|1\\rangle\\).

## Summary

Key concepts:

- CZ adds phase when both qubits are \\(|1\\rangle\\)
- SWAP exchanges qubit states
- Many gates are equivalent under basis transformations
- Gate decomposition matters for hardware implementation

With two-qubit gates mastered, you're ready to build complex circuits and patterns.

> **Official Documentation:** The CZ and SWAP gates are documented in the [Qiskit circuit library](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.CZGate) and [SWAPGate](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.SWAPGate).

______________________________________________________________________

## Try These On QCoder

- [QPC002 B3: SWAP Qubits](https://www.qcoder.jp/en/contests/QPC002/problems/B3)

______________________________________________________________________

*Next: [State Preparation Patterns](./state-preparation.md)*
