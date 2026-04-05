# Gate Basics: X, Y, Z

Now that you understand qubits and statevectors, let's learn the basic gates. These are the building blocks for all quantum circuits.

## What Is a Gate?

A quantum gate is a **unitary operation** that transforms a quantum state. Think of it as a function that maps input states to output states.

```
Input State ──[Gate]──> Output State
   |α⟩|0⟩ + |β⟩|1⟩          |α'⟩|0⟩ + |β'⟩|1⟩
```

Gates preserve the normalization (\\(|\\alpha|^2 + |\\beta|^2 = 1\\)) and are reversible.

## The Pauli Gates

The Pauli gates are the most fundamental single-qubit gates. They correspond to 180° rotations around the x, y, and z axes of the Bloch sphere.

### The X Gate (NOT)

The X gate flips \\(|0\\rangle\\) and \\(|1\\rangle\\):

\\[X|0\\rangle = |1\\rangle\\]
\\[X|1\\rangle = |0\\rangle\\]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# X gate on |0⟩ gives |1⟩
qc = QuantumCircuit(1)
qc.x(0)
print(Statevector.from_instruction(qc))
```

Output:

```
Statevector([0.+0.j, 1.+0.j],
            dims=(2,))
```

The X gate is the quantum equivalent of classical NOT, but with a catch: it's reversible.

### The Z Gate (Phase Flip)

The Z gate flips the sign of the \\(|1\\rangle\\) component:

\\[Z|0\\rangle = |0\\rangle\\]
\\[Z|1\\rangle = -|1\\rangle\\]

```python
# Z gate leaves |0⟩ unchanged
qc1 = QuantumCircuit(1)
qc1.z(0)
print("Z on |0⟩:", Statevector.from_instruction(qc1))

# Z gate flips sign of |1⟩
qc2 = QuantumCircuit(1)
qc2.x(0)  # Prepare |1⟩
qc2.z(0)  # Apply Z
print("Z on |1⟩:", Statevector.from_instruction(qc2))
```

Output:

```
Z on |0⟩: Statevector([1.+0.j, 0.+0.j], dims=(2,))
Z on |1⟩: Statevector([0.+0.j, -1.+0.j], dims=(2,))
```

The Z gate is invisible if your qubit is definitely in \\(|0\\rangle\\)—but becomes crucial in superposition states.

### The Y Gate

The Y gate does both: flips the state and adds a phase:

\\[Y|0\\rangle = i|1\\rangle\\]
\\[Y|1\\rangle = -i|0\\rangle\\]

```python
# Y on |0⟩
qc = QuantumCircuit(1)
qc.y(0)
print(Statevector.from_instruction(qc))
```

Output:

```
Statevector([0.+0.j, 0.+1.j],
            dims=(2,))
```

Note the \\(i\\) (imaginary unit) in the amplitude. This is fine—amplitudes can be complex.

## Visualizing the Pauli Gates

On the Bloch sphere, the Pauli gates are 180° rotations:

```
        |0⟩ (north pole)
           ↑
           │  X-axis
       ←────●─────→ rotation
           │
           ↓
        |1⟩ (south pole)
```

Let's see this in action:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# X gate: rotates from |0⟩ to |1⟩ around x-axis
qc_x = QuantumCircuit(1)
qc_x.x(0)
display(plot_bloch_multivector(Statevector.from_instruction(qc_x)))

# Z gate: leaves |0⟩ unchanged
qc_z = QuantumCircuit(1)
qc_z.z(0)
display(plot_bloch_multivector(Statevector.from_instruction(qc_z)))

# Z on |+⟩: rotates to |−⟩ (phase flip visible on equator)
qc_z_on_plus = QuantumCircuit(1)
qc_z_on_plus.h(0)
qc_z_on_plus.z(0)
display(plot_bloch_multivector(Statevector.from_instruction(qc_z_on_plus)))

# Y gate: interesting because it has imaginary amplitudes
qc_y = QuantumCircuit(1)
qc_y.y(0)
display(plot_bloch_multivector(Statevector.from_instruction(qc_y)))
```

| Gate | Rotation Axis | Angle |
|------|---------------|-------|
| X | x-axis | 180° |
| Y | y-axis | 180° |
| Z | z-axis | 180° |

## The Identity Gate

The identity gate (I) does nothing:

\\[I|0\\rangle = |0\\rangle\\]
\\[I|1\\rangle = |1\\rangle\\]

```python
qc = QuantumCircuit(1)
qc.i(0)  # Identity gate
```

You won't use this much, but it appears in theoretical discussions.

## Gate Composition

You can apply multiple gates in sequence:

```python
# X then X gives |0⟩ again (X² = I)
qc = QuantumCircuit(1)
qc.x(0)
qc.x(0)
print(Statevector.from_instruction(qc))

# X then Z
qc2 = QuantumCircuit(1)
qc2.x(0)
qc2.z(0)
print(Statevector.from_instruction(qc2))
```

**Key insight:** Gate order matters!

\\[XZ \\neq ZX\\]

```python
# Z then X (different from X then Z)
qc3 = QuantumCircuit(1)
qc3.z(0)
qc3.x(0)
print(Statevector.from_instruction(qc3))
```

Do you notice the difference?

## Checking Gate Equivalence

You can verify that two circuits are equivalent using `Operator`:

```python
from qiskit.quantum_info import Operator

qc_xz = QuantumCircuit(1)
qc_xz.x(0)
qc_xz.z(0)

qc_zx = QuantumCircuit(1)
qc_zx.z(0)
qc_zx.x(0)

print("XZ circuit:")
print(qc_xz)

print("ZX circuit:")
print(qc_zx)

print("Are they equal?", Operator(qc_xz) == Operator(qc_zx))
```

## Multi-Qubit Gates

The Pauli gates can also be applied to multiple qubits:

```python
qc = QuantumCircuit(3)
qc.x(0)   # Flip qubit 0
qc.y(1)   # Flip qubit 1 (with phase)
qc.z(2)   # Flip qubit 2 (with phase)
```

Each gate acts independently on its target qubit.

> **Official Documentation:** For more details on Pauli gates, see the [Qiskit documentation on single-qubit gates](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.UGate). See also [The Plus/Minus Basis](./single-qubit-toolbox.md) for the complete gate summary table.

## Checkpoint Exercises

### Exercise 1

Apply X to \\(|0\\rangle\\). What state do you get?

### Exercise 2

Apply Z to \\(|1\\rangle\\). What state do you get? Is it the same as \\(Z|0\\rangle\\)?

### Exercise 3

Apply X then Z to \\(|0\\rangle\\). Then apply Z then X to \\(|0\\rangle\\). Are the results the same?

### Exercise 4

Apply Y to \\(|0\\rangle\\). What do you notice about the amplitudes?

### Exercise 5

Verify that \\(X^2 = I\\) (applying X twice returns to the original state).

## The Next Gate: Hadamard

The Pauli gates are important, but they're limited: they only give basis states. The **Hadamard gate** creates superposition, which is where quantum computing gets interesting.

______________________________________________________________________

## Try These On QCoder

- [QPC001 A1: Generate Plus state](https://www.qcoder.jp/en/contests/QPC001/problems/A1)
- [QPC001 A5: Generate state II](https://www.qcoder.jp/en/contests/QPC001/problems/A5)

______________________________________________________________________

> **Official Documentation:** For more details on Pauli gates, see the [Qiskit documentation on single-qubit gates](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.UGate).

______________________________________________________________________

*Next: [Superposition: The Hadamard Gate](./single-qubit-gates-h.md)*
