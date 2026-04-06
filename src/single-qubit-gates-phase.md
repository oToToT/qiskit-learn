# Phase: The Z Gate and Relative Phase

The Z gate looked harmless when we first saw it. Now that you understand superposition, we can see its true power: the Z gate introduces **relative phase**, which is invisible in immediate measurements but crucial for interference.

## Global Phase vs Relative Phase

This distinction is fundamental.

### Global Phase

Multiplying the *entire state* by a complex number:

\\[|\\psi\\rangle \\rightarrow e^{i\\phi}|\\psi\\rangle\\]

**This changes nothing physically.** All measurement probabilities remain the same.

### Relative Phase

Multiplying *one amplitude* by a complex number:

\\[|0\\rangle + |1\\rangle \\rightarrow |0\\rangle - |1\\rangle\\]

**This changes everything physically.** The state is different, even if measurement statistics are the same.

## The Z Gate and Relative Phase

The Z gate applies a phase flip to the \\(|1\\rangle\\) component:

\\[Z(\\alpha|0\\rangle + \\beta|1\\rangle) = \\alpha|0\\rangle - \\beta|1\\rangle\\]

On basis states:

- \\(Z|0\\rangle = |0\\rangle\\) (unchanged)
- \\(Z|1\\rangle = -|1\\rangle\\) (phase flip)

## Why Z Seems Useless on \\(|0\\rangle\\)

When your qubit is definitely in \\(|0\\rangle\\), Z does nothing visible:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.z(0)
print(Statevector(qc))
```

Output:

```
Statevector([1.+0.j, 0.+0.j],
            dims=(2,))
```

The state is still \\(|0\\rangle\\). No change.

**But watch what happens after a Hadamard:**

```python
# Without Z
qc1 = QuantumCircuit(1)
qc1.h(0)
print("H only:", Statevector(qc1))

# With Z
qc2 = QuantumCircuit(1)
qc2.h(0)
qc2.z(0)
print("H then Z:", Statevector(qc2))
```

Output:

```
H only: Statevector([0.70710678+0.j, 0.70710678+0.j], dims=(2,))
H then Z: Statevector([0.70710678+0.j, -0.70710678+0.j], dims=(2,))
```

The states are different! The Z gate introduces a relative phase of \\(\\pi\\) (180°) between the two branches.

## The Minus State

The state \\(|-\\rangle = \\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}}\\) is created by `H` then `Z`:

```python
qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)
print(Statevector(qc))
```

Or equivalently by `X` then `H`:

```python
qc = QuantumCircuit(1)
qc.x(0)
qc.h(0)
print(Statevector(qc))
```

Both give \\(|-\\rangle\\). Verify this!

## Visualizing Phase on the Bloch Sphere

On the Bloch sphere, phase corresponds to position around the equator:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector
from math import pi

# |+⟩ - phase 0
qc = QuantumCircuit(1)
qc.h(0)
display(plot_bloch_multivector(Statevector(qc)))

# |−⟩ - phase π (180°)
qc = QuantumCircuit(1)
qc.x(0)
qc.h(0)
display(plot_bloch_multivector(Statevector(qc)))

# P(π/2) - phase π/2
qc = QuantumCircuit(1)
qc.p(pi/2, 0)
display(plot_bloch_multivector(Statevector(qc)))

# P(π/4) - phase π/4
qc = QuantumCircuit(1)
qc.p(pi/4, 0)
display(plot_bloch_multivector(Statevector(qc)))
```

```
           |+⟩ (phase 0)
            ●
           / \
          /   \
    |−⟩──●     ●──|+i⟩
         \   /
          \ /
           ●
          |0⟩
```

- \\(|+\\rangle\\) has phase 0
- \\(|-\\rangle\\) has phase π (180°)
- \\(|+i\\rangle\\) and \\(|-i\\rangle\\) are eigenstates of Y (phase π/2 and 3π/2)

## Phase in Multi-Qubit States

Relative phase becomes more interesting with multiple qubits:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create superposition on two qubits
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)

print("Equal superposition:", Statevector(qc))

# Now apply Z to qubit 0 only
qc2 = QuantumCircuit(2)
qc2.h(0)
qc2.h(1)
qc2.z(0)

print("With Z on q0:", Statevector(qc2))
```

The \\(|1\\rangle\\) component on qubit 0 gets the phase flip, creating entanglement between the phase and which qubit is in state 1.

## CZ Gate: Phase on Two Qubits

The controlled-Z (CZ) gate adds a phase flip when *both* qubits are \\(|1\\rangle\\):

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create superposition on both qubits
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)

print("Before CZ:", Statevector(qc))

# Apply CZ
qc.cz(0, 1)

print("After CZ:", Statevector(qc))
```

Output shows a phase flip on the \\(|11\\rangle\\) component.

## Why Phase Matters

Phase is invisible in immediate measurements but matters because:

1. **Interference**: Phases combine when states recombine
1. **Entanglement**: Phase correlations create entangled states
1. **Quantum algorithms**: Phase manipulation is the core of most algorithms

Example: Grover's algorithm uses phase differences to amplify the probability of marked states.

## Measuring Phase

You can't directly "see" relative phase in a single measurement. But you can *reveal* it with basis changes:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.primitives import StatevectorSampler

# Create |−⟩
qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)

# Measure in X basis (apply H then measure)
qc.h(0)  # Basis change: Z basis → X basis
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=100).result()
print(result[0].data.meas.get_counts())
```

Output: Always `1` (never `0`).

Why? Because \\(|-\\rangle\\) *is* the \\(|1\\rangle\\) state in the X basis. The phase is revealed by changing the measurement basis.

## The Phase Gate (P)

Qiskit has a general phase gate `p(theta)` that adds an arbitrary phase:

\\[P(\\theta)|1\\rangle = e^{i\\theta}|1\\rangle\\]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from math import pi

# P(π) should equal Z
qc = QuantumCircuit(1)
qc.p(pi, 0)
print("P(π):", Statevector(qc))

# P(π/2) adds 90° phase
qc2 = QuantumCircuit(1)
qc2.p(pi/2, 0)
print("P(π/2):", Statevector(qc2))
```

## Checkpoint Exercises

### Exercise 1

Show that \\(Z = PHP(\\pi)\\) where H is the Hadamard gate in the X basis. (Hint: use `Operator` to compare)

### Exercise 2

Create \\(|-\\rangle\\) three different ways and verify they give identical states.

### Exercise 3

Apply Z to a qubit in superposition. Show that the probabilities don't change but the state vector does.

### Exercise 4

Use the CZ gate to create phase entanglement between two qubits.

### Exercise 5

Design a circuit that distinguishes \\(|+\\rangle\\) from \\(|-\\rangle\\) without destroying the state.

## Summary

Key concepts:

- Global phase (\\(e^{i\\phi}|\\psi\\rangle\\)) is physically irrelevant
- Relative phase between amplitudes is physically meaningful
- Z gate introduces relative phase of π on \\(|1\\rangle\\)
- Phase is invisible in immediate measurement but visible in interference
- Basis changes reveal hidden phase information

Understanding phase is essential for quantum computing. Many algorithms (Grover, QFT, phase estimation) are fundamentally about manipulating phases.

> **Official Documentation:** For more on phase and phase gates, see IBM's [phase kickback tutorial](https://learning.quantum.ibm.com/tutorial/phase-kickback-and-phase-rotations) which explains how phase is used in quantum algorithms.

______________________________________________________________________

## Try These On QCoder

- [QPC002 A1: Generate state (−|0⟩−|0⟩)](https://www.qcoder.jp/en/contests/QPC002/problems/A1)
- [QPC002 B1: Generate State e^(iθ)|0⟩](https://www.qcoder.jp/en/contests/QPC002/problems/B1)
- [QPC002 B2: Phase Shift Oracle](https://www.qcoder.jp/en/contests/QPC002/problems/B2)

______________________________________________________________________

*Next: [Rotations: RX, RY, RZ, and P](./single-qubit-gates-rotations.md)*
