# Superposition: The Hadamard Gate

The Hadamard gate is the key to quantum computing. It transforms a definite state into a superposition—allowing a qubit to be "both 0 and 1 at the same time."

## The Basic Action

The Hadamard gate (H) transforms the basis states:

\\[H|0\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}} = |+\\rangle\\]
\\[H|1\\rangle = \\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}} = |-\\rangle\\]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# H on |0⟩
qc = QuantumCircuit(1)
qc.h(0)
print("H|0⟩:", Statevector(qc))

# H on |1⟩
qc2 = QuantumCircuit(1)
qc2.x(0)  # Prepare |1⟩
qc2.h(0)  # Apply H
print("H|1⟩:", Statevector(qc2))
```

Output:

```
H|0⟩: Statevector([ 0.70710678+0.j,  0.70710678+0.j], dims=(2,))
H|1⟩: Statevector([ 0.70710678+0.j, -0.70710678+0.j], dims=(2,))
```

## What Is \\(|+\\rangle\\) and \\(|-\\rangle\\)?

These are the **plus and minus states**—eigenstates in a different basis:

| State | Name | Definition |
|-------|------|------------|
| \\(|+\\rangle\\) | Plus state | \\(\\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\) |
| \\(|-\\rangle\\) | Minus state | \\(\\frac{|0\\rangle-|1\\rangle}{\\sqrt{2}}\\) |

The \\(|+\\rangle\\) and \\(|-\\rangle\\) states are:

- **Orthogonal**: \\(\\langle +|- \\rangle = 0\\)
- **Normalized**: \\(|+\\rangle = 1\\)
- **Complete**: Any single-qubit state can be written as a combination of them

## The Matrix Form

The Hadamard matrix is:

\\[H = \\frac{1}{\\sqrt{2}}\\begin{pmatrix} 1 & 1 \\\\ 1 & -1 \\end{pmatrix}\\]

Applying this to \\(|0\\rangle = \\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix}\\):

\\[H|0\\rangle = \\frac{1}{\\sqrt{2}}\\begin{pmatrix} 1 & 1 \\\\ 1 & -1 \\end{pmatrix}\\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix} = \\frac{1}{\\sqrt{2}}\\begin{pmatrix} 1 \\\\ 1 \\end{pmatrix} = |+\\rangle\\]

## Creating Superposition on Multiple Qubits

To create equal superposition on n qubits, apply H to each:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Equal superposition on 2 qubits
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)
print(Statevector(qc))
```

Output:

```
Statevector([0.5+0.j, 0.5+0.j, 0.5+0.j, 0.5+0.j],
            dims=(2, 2))
```

This is \\(\\frac{|00\\rangle + |01\\rangle + |10\\rangle + |11\\rangle}{2}\\)—equal superposition over all 2-qubit basis states.

For n qubits: equal superposition gives \\(\\frac{1}{\\sqrt{2^n}}\\sum\_{x}|x\\rangle\\)

## Superposition Is Not Mixture

A common misconception: superposition means "the qubit is randomly 0 or 1."

**This is wrong.**

Superposition is a *coherent* combination where phases matter. Consider:

\\[|+\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\]

\\[|-\\rangle = \\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}}\\]

Both give 50-50 measurement outcomes, but they are *different* quantum states. The difference only appears in interference effects.

## Measuring Superposition States

When you measure a qubit in superposition:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(1)
qc.h(0)
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()
print(result[0].data.meas.get_counts())
```

Output (approximately):

```
{'0': 498, '1': 502}
```

Measurement collapses the state randomly, with probabilities determined by the amplitudes.

## The Power of Superposition

Here's why superposition matters: with n qubits in superposition, you have a quantum state that represents *all* \\(2^n\\) basis states simultaneously.

```
1 qubit:  2 states at once
2 qubits: 4 states at once
3 qubits: 8 states at once
...
10 qubits: 1,024 states at once
20 qubits: 1,048,576 states at once
```

This isn't just parallel computation—it's a *superposition* where amplitudes can interfere constructively or destructively.

## The Hadamard as Basis Change

Think of H as a **basis change**:

```
Computational Basis     ←→     Plus/Minus Basis
    |0⟩                      |+⟩
    |1⟩                      |−⟩
```

This is crucial: gates in different bases do different things.

## Visualizing Superposition

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# |0⟩ before H - at north pole
qc = QuantumCircuit(1)
display(plot_bloch_multivector(Statevector(qc)))

# After H - |+⟩ on the equator!
qc = QuantumCircuit(1)
qc.h(0)
display(plot_bloch_multivector(Statevector(qc)))

# H on |1⟩ gives |−⟩ - opposite on equator
qc = QuantumCircuit(1)
qc.x(0)
qc.h(0)
display(plot_bloch_multivector(Statevector(qc)))

# Equal superposition on 2 qubits - shows both on Bloch spheres
qc = QuantumCircuit(2)
qc.h(0)
qc.h(1)
display(plot_bloch_multivector(Statevector(qc)))
```

## Applying H Twice

What happens when you apply H twice?

\\[HH|0\\rangle = HH|0\\rangle = |0\\rangle\\]

```python
from qiskit.quantum_info import Operator

qc = QuantumCircuit(1)
qc.h(0)
qc.h(0)

print("H twice:", Operator(qc).data)
print("Identity?", Operator(qc).data.round(5))
```

Output:

```
H twice: [[ 1.00000e+00+0.j  -5.55112e-17+0.j]
          [-5.55112e-17+0.j  1.00000e+00+0.j]]
Identity? True
```

Yes! \\(H^2 = I\\) (identity). This is because H is its own inverse.

## Basis Changes in Practice

A key insight: applying H transforms computational-basis operations to plus/minus-basis operations:

\\[HZH = X\\]
\\[HXH = Z\\]
\\[HYH = -Y\\]

This identity lets you implement operations in different bases. We'll use this repeatedly.

## Checkpoint Exercises

### Exercise 1

Create superposition on 1 qubit. Verify the amplitudes are both \\(1/\\sqrt{2}\\).

### Exercise 2

Create superposition on 3 qubits. How many states are in superposition?

### Exercise 3

Apply H twice to \\(|1\\rangle\\). What state do you get?

### Exercise 4

Create \\(|-\\rangle\\) starting from \\(|0\\rangle\\).

### Exercise 5

Verify that \\(HZH = X\\) using Qiskit.

## Summary

Key takeaways:

- H creates superposition from basis states
- \\(H|0\\rangle = |+\\rangle\\) and \\(H|1\\rangle = |-\\rangle\\)
- H is its own inverse: \\(H^2 = I\\)
- Superposition is coherent, not random
- Multi-qubit superposition: apply H to each qubit
- H changes between computational and plus/minus bases

The Hadamard gate is your gateway to quantum computation. With H, you can create any superposition state.

> **Official Documentation:** The Hadamard gate is part of Qiskit's [circuit library](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.HGate). For a deeper look at superposition, see IBM's [superposition tutorial](https://learning.quantum.ibm.com/tutorial/introduction-to-quantum-superposition).

______________________________________________________________________

*Next: [Phase: The Z Gate and Relative Phase](./single-qubit-gates-phase.md)*
