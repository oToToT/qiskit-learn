# Rotations: RX, RY, RZ, and P

The Pauli gates (X, Y, Z) rotate by 180°. The rotation gates let you rotate by arbitrary angles—giving you precise control over quantum states.

## Rotation Gates Overview

| Gate | Axis | Description |
|------|------|-------------|
| `rx(theta)` | x-axis | Rotation around the x-axis |
| `ry(theta)` | y-axis | Rotation around the y-axis |
| `rz(theta)` | z-axis | Rotation around the z-axis |
| `p(theta)` | z-axis | Phase (relative to |0⟩) |

All angles are in **radians**, not degrees.

## The RX Gate (X-Axis Rotation)

\\(R_x(\\theta)\\) rotates around the x-axis:

\\[R_x(\\theta)|0\\rangle = \\cos(\\frac{\\theta}{2})|0\\rangle - i\\sin(\\frac{\\theta}{2})|1\\rangle\\]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from math import pi

# 180° rotation (should equal X)
qc = QuantumCircuit(1)
qc.rx(pi, 0)
print("RX(π):", Statevector(qc))
```

Output:

```
RX(π): Statevector([ 6.123234e-17+0.j, -6.123234e-17-1.j],
            dims=(2,))
```

The amplitude on \\(|1\\rangle\\) is approximately \\(-i\\), which has the same effect as \\(-1\\) up to global phase. So \\(R_x(\\pi) \\approx X\\).

## The RY Gate (Y-Axis Rotation)

\\(R_y(\\theta)\\) rotates around the y-axis. This is often the most intuitive rotation because it directly controls the probability of measuring \\(|0\\rangle\\) vs \\(|1\\rangle\\):

\\[R_y(\\theta)|0\\rangle = \\cos(\\frac{\\theta}{2})|0\\rangle + \\sin(\\frac{\\theta}{2})|1\\rangle\\]

```python
# RY(π/2) should give 50-50 superposition
qc = QuantumCircuit(1)
qc.ry(pi/2, 0)
print("RY(π/2):", Statevector(qc))
```

Output:

```
RY(π/2): Statevector([0.70710678+0.j, 0.70710678+0.j], dims=(2,))
```

The amplitudes are both \\(1/\\sqrt{2}\\)—exactly like the Hadamard!

## The Half-Angle Pattern

Notice the pattern: \\(R_y(\\theta)\\) uses \\(\\theta/2\\) in the formulas. This is because:

- A full rotation (\\(2\\pi\\)) should return to the same state
- So the rotation angle in the formulas is \\(\\theta/2\\)

| Rotation Angle | Effect |
|----------------|--------|
| \\(R_y(\\pi)\\) | \\(|0\\rangle \\rightarrow |1\\rangle\\) (bit flip) |
| \\(R_y(\\pi/2)\\) | \\(|0\\rangle \\rightarrow \\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\) (superposition) |
| \\(R_y(\\pi/4)\\) | Partial rotation toward \\(|1\\rangle\\) |

## Designing States with RY

The RY gate is your main tool for preparing states with specific probabilities:

```python
# Want P(|1⟩) = 3/4
# That means sin²(θ/2) = 3/4
# So θ/2 = arcsin(√(3/4)) = π/3
# And θ = 2π/3

from math import pi, asin, sqrt

target_prob = 3/4
theta = 2 * asin(sqrt(target_prob))

qc = QuantumCircuit(1)
qc.ry(theta, 0)
state = Statevector(qc)
print("State:", state)

# Verify the probability
probs = state.probabilities()
print("P(|1⟩) =", probs[1])
```

Output:

```
State: Statevector([0.5+0.j, 0.8660254+0.j], dims=(2,))
P(|1⟩) = 0.7499999999999999
```

The probability is approximately 0.75, as desired.

## The RZ Gate (Z-Axis Rotation)

\\(R_z(\\theta)\\) rotates around the z-axis:

\\[R_z(\\theta)|0\\rangle = |0\\rangle\\]
\\[R_z(\\theta)|1\\rangle = e^{i\\theta}|1\\rangle\\]

```python
# RZ adds phase to |1⟩
qc = QuantumCircuit(1)
qc.x(0)  # Start with |1⟩
qc.rz(pi/4, 0)  # Add π/4 phase
print(Statevector(qc))
```

Note: \\(R_z\\) doesn't change the probabilities—only the phase.

## The Phase Gate (P)

The `p(theta)` gate is equivalent to \\(R_z(\\theta)\\):

\\[P(\\theta) = R_z(\\theta)\\]

```python
qc = QuantumCircuit(1)
qc.x(0)
qc.p(pi/2, 0)  # Add π/2 phase to |1⟩
print(Statevector(qc))
```

## Rotations in Practice

### Creating Any Single-Qubit State

Any single-qubit state can be created with two rotations:

```python
# Create a state with specific amplitudes (up to global phase)
alpha = 0.6  # amplitude for |0⟩
beta = 0.8   # amplitude for |1⟩

# From |0⟩, we need:
# R_y(θ)|0⟩ = cos(θ/2)|0⟩ + sin(θ/2)|1⟩
# So sin(θ/2) = |beta|, cos(θ/2) = |alpha|
# θ = 2 * atan2(|beta|, |alpha|)

from math import atan2, sqrt

theta = 2 * atan2(abs(beta), abs(alpha))

qc = QuantumCircuit(1)
qc.ry(theta, 0)

# Add phase if needed (using rz)
phi = 0  # phase on |1⟩
qc.rz(phi, 0)

print(Statevector(qc))
```

## Visualizing Rotation Gates

The rotation gates trace paths on the Bloch sphere:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector
from math import pi

# RY rotates between |0⟩ and |1⟩ (controls probability)
for angle in [0, pi/4, pi/2, 3*pi/4, pi]:
    qc = QuantumCircuit(1)
    qc.ry(angle, 0)
    display(plot_bloch_multivector(Statevector(qc)))

# RZ rotates around the equator (controls phase only)
# Note: RZ on |0⟩ has no visible effect on Bloch sphere
qc = QuantumCircuit(1)
qc.rz(pi/2, 0)
print("RZ(π/2) on |0⟩:", Statevector(qc))
display(plot_bloch_multivector(Statevector(qc)))

# RZ on |+⟩ - shows phase change
qc = QuantumCircuit(1)
qc.h(0)
display(plot_bloch_multivector(Statevector(qc)))

qc.rz(pi/2, 0)
display(plot_bloch_multivector(Statevector(qc)))
```

Key insight:

- **RY moves the state between poles** (changes measurement probabilities)
- **RZ rotates around the equator** (changes phase without changing probabilities)

## Rotation Summary

| Gate | Matrix | Action |
|------|--------|--------|
| \\(R_x(\\theta)\\) | \\(\\begin{pmatrix} \\cos\\frac{\\theta}{2} & -i\\sin\\frac{\\theta}{2} \\\\ -i\\sin\\frac{\\theta}{2} & \\cos\\frac{\\theta}{2} \\end{pmatrix}\\) | Rotate around x-axis |
| \\(R_y(\\theta)\\) | \\(\\begin{pmatrix} \\cos\\frac{\\theta}{2} & -\\sin\\frac{\\theta}{2} \\\\ \\sin\\frac{\\theta}{2} & \\cos\\frac{\\theta}{2} \\end{pmatrix}\\) | Rotate around y-axis |
| \\(R_z(\\theta)\\) | \\(\\begin{pmatrix} e^{-i\\theta/2} & 0 \\\\ 0 & e^{i\\theta/2} \\end{pmatrix}\\) | Rotate around z-axis |
| \\(P(\\theta)\\) | \\(\\begin{pmatrix} 1 & 0 \\ 0 & e^{i\\theta} \\end{pmatrix}\\) | Add phase to |1⟩ |

## Useful Identities

| Identity | Meaning |
|----------|---------|
| \\(R_x(\\pi) = X\\) | 180° x-rotation = X gate |
| \\(R_y(\\pi) = Y\\) | 180° y-rotation = Y gate |
| \\(R_z(\\pi) = Z\\) | 180° z-rotation = Z gate |
| \\(R_y(\\pi/2) \\approx H\\) | 90° y-rotation ≈ Hadamard (up to phase) |
| \\(R_x(\\theta) = HR_z(\\theta)H\\) | X-basis rotations via basis change |

## Checkpoint Exercises

### Exercise 1

Use RY to create a state with P(|0⟩) = 1 (the |0⟩ state). What angle do you need?

### Exercise 2

Use RY to create a state with P(|1⟩) = 0.25.

### Exercise 3

Verify that RY(π) flips |0⟩ to |1⟩.

### Exercise 4

Create a state with P(|1⟩) = 0.5 and a relative phase of π/2 on the |1⟩ component.

### Exercise 5

Explain why RZ doesn't change measurement probabilities.

## Summary

Key takeaways:

- Rotation gates give fine-grained control over quantum states
- \\(R_x\\), \\(R_y\\), \\(R_z\\) rotate around different axes by angle θ
- The formula uses θ/2 due to the geometry of the Bloch sphere
- RY is best for controlling measurement probabilities
- RZ controls phase without changing probabilities
- P(θ) is equivalent to RZ(θ)

These gates give you the tools to create any single-qubit quantum state.

> **Official Documentation:** Rotation gates are documented in the [Qiskit circuit library](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.RXGate). The [PhaseGate (P)](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.PhaseGate) is equivalent to RZ. For the math behind rotations, see IBM's [transpilation documentation on gate decomposition](https://docs.quantum.ibm.com/build/transpile).

______________________________________________________________________

*Next: [The Plus/Minus Basis and Basis Changes](./single-qubit-toolbox.md)*
