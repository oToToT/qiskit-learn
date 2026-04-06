# The Plus/Minus Basis and Basis Changes

This chapter ties together everything you've learned about single-qubit gates and introduces the powerful concept of basis changes—a mental model that underlies many quantum algorithms.

## The Four Cardinal Bases

Every single-qubit state can be expressed in any of these bases:

| Basis | States | Created By |
|-------|--------|------------|
| Computational (Z) | \\(|0\\rangle\\), \\(|1\\rangle\\) | \\(I\\), \\(X\\) |
| Plus/Minus (X) | \\(|+\\rangle\\), \\(|-\\rangle\\) | \\(H\\), \\(HX\\) |
| Circular (Y) | \\(|+i\\rangle\\), \\(|-i\\rangle\\) | \\(HS^\\dagger\\), etc. |

Understanding how to switch between bases is crucial for quantum algorithms.

## The Computational Basis (Z Basis)

The default basis:

- Eigenstates of the Z operator
- \\(|0\\rangle\\) and \\(|1\\rangle\\)
- Created by: \\(I\\) (for \\(|0\\rangle\\)), \\(X\\) (for \\(|1\\rangle\\))

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# |0⟩ in Z basis
qc = QuantumCircuit(1)
print("|0⟩:", Statevector(qc))

# |1⟩ in Z basis
qc.x(0)
print("|1⟩:", Statevector(qc))
```

## The Plus/Minus Basis (X Basis)

Eigenstates of the X operator:

- \\(|+\\rangle = \\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\)
- \\(|-\\rangle = \\frac{|0\\rangle-|1\\rangle}{\\sqrt{2}}\\)
- Created by: \\(H\\) (for \\(|+\\rangle\\)), \\(XH\\) or \\(HX\\) (for \\(|-\\rangle\\))

```python
# |+⟩ in X basis
qc = QuantumCircuit(1)
qc.h(0)
print("|+⟩:", Statevector(qc))

# |−⟩ in X basis
qc.x(0)
qc.h(0)
print("|−⟩:", Statevector(qc))
```

## The Key Identity: HZH = X

This identity reveals the connection between bases:

\\[HZH = X\\]

What does this mean?

- \\(H\\) changes from Z basis to X basis
- \\(Z\\) acts (phase flip)
- \\(H\\) changes back from X basis to Z basis
- Net effect: bit flip!

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Operator

# Build HZH
qc_hzh = QuantumCircuit(1)
qc_hzh.h(0)
qc_hzh.z(0)
qc_hzh.h(0)

# Build X
qc_x = QuantumCircuit(1)
qc_x.x(0)

# Are they equal?
print("HZH == X:", Operator(qc_hzh) == Operator(qc_x))
```

## General Basis Change Formula

In general, to transform an operation from one basis to another:

\\[B \\cdot \\text{Operation} \\cdot B^\\dagger = \\text{New Operation}\\]

Where \\(B\\) is the basis change operation.

For the X-Z connection: \\(HZH = X\\)

## Basis Changes in Practice

### Problem: Apply X in the X basis

In the X basis, the eigenstates are \\(|+\\rangle\\) and \\(|-\\rangle\\). But we want to "flip" \\(|+\\rangle\\) to \\(|-\\rangle\\).

Solution: Change to Z basis, flip, change back.

```python
# Flip |+⟩ to |−⟩
qc = QuantumCircuit(1)

# Start with |+⟩
qc.h(0)

# Change to Z basis, apply X, change back
qc.h(0)  # |+⟩ → |0⟩
qc.x(0)  # |0⟩ → |1⟩
qc.h(0)  # |1⟩ → |−⟩

print(Statevector(qc))
```

This is equivalent to applying Z in the original basis!

## The Complete Picture: All Pauli Conjugates

The Hadamard gate transforms all Pauli gates:

| Gate | Transform |
|------|-----------|
| \\(H X H = Z\\) | X ↔ Z |
| \\(H Y H = -Y\\) | Y stays Y (up to global phase) |
| \\(H Z H = X\\) | Z ↔ X |

```python
from qiskit.quantum_info import Operator

# Verify all three
for op, name in [('x', 'X'), ('y', 'Y'), ('z', 'Z')]:
    qc = QuantumCircuit(1)
    qc.h(0)
    getattr(qc, op)(0)
    qc.h(0)
    
    x_qc = QuantumCircuit(1)
    if name == 'Y':
        x_qc.y(0)
        x_qc.x(0)
    else:
        x_qc.x(0)
    
    # We expect X → Z, Z → X, Y → Y
    print(f"H {name} H =", Operator(qc).simplify())
```

## Why Basis Changes Matter

Basis changes are central to many quantum algorithms:

1. **Phase kickback**: Use basis changes to move phase information
1. **Measurement basis**: Measure in different bases to extract different information
1. **Algorithm design**: QFT, Grover, and others use basis changes extensively

## Measuring in Different Bases

You can measure in any basis by changing to that basis first:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

# Measure |+⟩ in Z basis: 50-50
qc = QuantumCircuit(1)
qc.h(0)  # Create |+⟩
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=100).result()
print("|+⟩ in Z basis:", result[0].data.meas.get_counts())

# Measure |+⟩ in X basis: always 0 (because |+⟩ is |0⟩ in X basis)
qc2 = QuantumCircuit(1)
qc2.h(0)  # Create |+⟩
qc2.h(0)  # Change to X basis for measurement
qc2.measure_all()

result2 = sampler.run([qc2], shots=100).result()
print("|+⟩ in X basis:", result2[0].data.meas.get_counts())
```

## Summary of Single-Qubit Mastery

You now know all the single-qubit tools:

| Gate | Code | Purpose |
|------|------|---------|
| X | `x(q)` | Bit flip |
| Y | `y(q)` | Bit + phase flip |
| Z | `z(q)` | Phase flip |
| H | `h(q)` | Create superposition / basis change |
| P | `p(θ, q)` | Add phase |
| RX | `rx(θ, q)` | Rotation around x-axis |
| RY | `ry(θ, q)` | Rotation around y-axis |
| RZ | `rz(θ, q)` | Rotation around z-axis |

## Checkpoint Exercises

### Exercise 1

Create \\(|-\\rangle\\) starting from \\(|0\\rangle\\) using only \\(H\\) and \\(Z\\).

### Exercise 2

Verify that \\(HZH = X\\) on both \\(|0\\rangle\\) and \\(|1\\rangle\\).

### Exercise 3

Measure \\(|-\\rangle\\) in the Z basis. What do you expect? Verify with code.

### Exercise 4

Design a circuit that flips \\(|+\\rangle\\) to \\(|-\\rangle\\).

### Exercise 5

Explain in your own words why \\(HZH = X\\).

## Summary

Key concepts:

- Four main bases: Z (computational), X (plus/minus), Y (circular)
- \\(H\\) connects Z and X bases
- \\(HZH = X\\) shows the relationship between operations in different bases
- You can measure in any basis by changing bases first
- Basis changes are fundamental to quantum algorithms

With single-qubit gates mastered, you're ready for the multi-qubit world.

______________________________________________________________________

## Try These On QCoder

- [QPC001 A3: Generate Minus state](https://www.qcoder.jp/en/contests/QPC001/problems/A3)
- [QPC002 B1: Generate State e^(i theta)|0>](https://www.qcoder.jp/en/contests/QPC002/problems/B1)
- [QPC003 B3: Generate State tensor_i (cos T_i |0> + sin T_i |1>)](https://www.qcoder.jp/en/contests/QPC003/problems/B3)

______________________________________________________________________

*Next: [Controlled Operations: CNOT](./two-qubits.md)*
