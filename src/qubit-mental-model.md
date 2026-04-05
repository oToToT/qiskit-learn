# What Is a Qubit?

Before writing more circuits, let's build a mental model for what a qubit *is*. This chapter is conceptual—it won't teach you new code, but it will help everything else make sense.

## The Classical Bit

You already know about classical bits. A bit is:

- Either `0` or `1`
- The fundamental unit of classical computing
- Readable, writable, copyable

```
Classical Bit:
┌─────────┐
│    0    │  ← One of two states
└─────────┘
```

## The Quantum Bit (Qubit)

A qubit is a quantum system with two basis states, but it can also exist in *superpositions* of those states.

The basis states are labeled \\(|0\\rangle\\) and \\(|1\\rangle\\) (using Dirac notation).

```
Qubit:
┌─────────────────────────────────────┐
│                                     │
│   α|0⟩ + β|1⟩                       │
│                                     │
│   where |α|² + |β|² = 1             │
│                                     │
└─────────────────────────────────────┘
```

The numbers \\(\\alpha\\) and \\(\\beta\\) are called **amplitudes**. They can be complex numbers.

## The Bloch Sphere

One common way to visualize a single qubit is the Bloch sphere. Every pure single-qubit state corresponds to a point on this sphere.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# Visualize |0⟩
qc = QuantumCircuit(1)
sv = Statevector.from_instruction(qc)
display(plot_bloch_multivector(sv))
```

```
        |0⟩ (north pole, θ=0)
         │
         │        z-axis
         │
         ●─────────────── x-axis
        /│\
       / │ \
      /  │  \
    |1⟩ │   |+⟩ (equator)
        \|/
```

Key points:

- \\(|0\\rangle\\) is at the north pole (θ=0)
- \\(|1\\rangle\\) is at the south pole (θ=π)
- \\(|+\\rangle = \\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\) is on the equator (φ=0)
- \\(|-\\rangle = \\frac{|0\\rangle-|1\\rangle}{\\sqrt{2}}\\) is also on the equator (φ=π)

Try visualizing different states:

```python
from qiskit.visualization import plot_bloch_multivector

# |1⟩ - south pole
qc = QuantumCircuit(1)
qc.x(0)
sv = Statevector.from_instruction(qc)
display(plot_bloch_multivector(sv))

# |+⟩ - on the equator
qc = QuantumCircuit(1)
qc.h(0)
sv = Statevector.from_instruction(qc)
display(plot_bloch_multivector(sv))

# |−⟩ - opposite on the equator
qc = QuantumCircuit(1)
qc.x(0)
qc.h(0)
sv = Statevector.from_instruction(qc)
display(plot_bloch_multivector(sv))
```

> **Note:** `plot_bloch_multivector` displays qubit states as vectors on the Bloch sphere. The vector points to the state on the sphere surface.

## Three Ways to Think About a Qubit

### 1. The Coin Analogy

Think of a qubit as a coin:

| Classical | Quantum |
|-----------|---------|
| Heads or Tails | \\(|0\\rangle\\) or \\(|1\\rangle\\) |
| The coin is one or the other | The coin can be in a superposition |
| Flipping reveals the result | Measuring forces a choice |

But this analogy has limits—real qubits have phases that coins don't.

### 2. The State Vector

Mathematically, a qubit is a 2-dimensional complex vector:

\\[|\\psi\\rangle = \\begin{pmatrix} \\alpha \\\\ \\beta \\end{pmatrix} = \\alpha|0\\rangle + \\beta|1\\rangle\\]

With the constraint: \\(|\\alpha|^2 + |\\beta|^2 = 1\\)

The constraint comes from probability: the probabilities of measuring \\(|0\\rangle\\) and \\(|1\\rangle\\) must add up to 1.

### 3. The Probability Distribution

The amplitudes encode probabilities:

- Probability of measuring \\(|0\\rangle\\): \\(|\\alpha|^2\\)
- Probability of measuring \\(|1\\rangle\\): \\(|\\beta|^2\\)

Example: For \\(|\\psi\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\):

- \\(|\\alpha|^2 = |\\frac{1}{\\sqrt{2}}|^2 = \\frac{1}{2}\\)
- \\(|\\beta|^2 = |\\frac{1}{\\sqrt{2}}|^2 = \\frac{1}{2}\\)

So you have a 50-50 chance of measuring either state.

## The Phase Is Also Important

Amplitudes can be complex, which means they have a **phase**:

\\[|\\psi\\rangle = \\alpha|0\\rangle + \\beta|1\\rangle\\]

Two states with the same probabilities but different phases:

- \\(|+\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\) (phases are the same)
- \\(|-\\rangle = \\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}}\\) (phases differ by \\(\\pi\\))

These measure the same (50-50) but behave differently when combined with other gates.

## Measurement

When you measure a qubit:

1. You get either \\(|0\\rangle\\) or \\(|1\\rangle\\)
1. The probability of each outcome is determined by the amplitudes
1. The state "collapses" to the measured value
1. Superposition is destroyed

```
Before measurement:  α|0⟩ + β|1⟩
                          ↓
                      MEASURE
                          ↓
After measurement:    |0⟩  (with probability |α|²)
              or     |1⟩  (with probability |β|²)
```

## Why Qubits Matter

Here's why qubits can do things classical bits can't:

1. **Superposition**: A qubit can be in a combination of \\(|0\\rangle\\) and \\(|1\\rangle\\)
1. **Entanglement**: Multiple qubits can be correlated in ways classical bits can't be
1. **Interference**: Phases can combine to amplify or cancel outcomes

These properties are the foundation for quantum algorithms.

## Gates Are Unitary Operations

In classical computing, gates (like NOT) are functions that map bits to bits.

In quantum computing, gates are **unitary operations**—matrices that:

- Preserve the normalization constraint (\\(|\\alpha|^2 + |\\beta|^2 = 1\\))
- Are reversible
- Transform the state vector

The Hadamard gate (H) transforms:
\\[H|0\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\]

\\[H|1\\rangle = \\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}}\\]

## Qubits vs Classical Bits: Key Differences

| Property | Classical Bit | Qubit |
|----------|--------------|-------|
| States | 0 or 1 | Any normalized superposition |
| Copying | Easy | Impossible (no-cloning theorem) |
| Reading | Destructive? | Destructive (collapses state) |
| Operations | Boolean logic | Unitary matrices |
| Determinism | Always deterministic | Probabilistic (when measured) |

## A Note on Notation

This book uses Dirac notation throughout. Here's a quick reference:

| Notation | Meaning |
|----------|---------|
| \\(|0\\rangle\\) | The "zero" basis state (column vector \\(\\begin{pmatrix} 1 \\\\ 0 \\end{pmatrix}\\)) |
| \\(|1\\rangle\\) | The "one" basis state (column vector \\(\\begin{pmatrix} 0 \\\\ 1 \\end{pmatrix}\\)) |
| \\(|\\psi\\rangle\\) | A quantum state named "psi" |
| \\(\\langle\\psi|\\) | The bra (conjugate transpose of \\(|\\psi\\rangle\\)) |
| \\(\\langle\\psi|\\phi\\rangle\\) | Inner product |
| \\(|+\\rangle\\) | The plus state \\(\\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\) |
| \\(|-\\rangle\\) | The minus state \\(\\frac{|0\\rangle-|1\\rangle}{\\sqrt{2}}\\) |

## Summary

A qubit is a two-level quantum system that:

- Has two basis states: \\(|0\\rangle\\) and \\(|1\\rangle\\)
- Can exist in superpositions: \\(\\alpha|0\\rangle + \\beta|1\\rangle\\)
- Has amplitudes that encode probabilities and phases
- Collapses upon measurement
- Is transformed by unitary gates

Understanding qubits as vectors in a 2D complex space will make everything in this book click.

## Quick Self-Check Quiz

Test your understanding with these questions:

<details>
<summary>Q1: If a qubit state is \\(|ψ⟩ = \frac{1}{2}|0⟩ + \frac{\sqrt{3}}{2}|1⟩\\), what is P(|0⟩)?</summary>

**Answer:** \\(|1/2|² = 1/4 = 0.25\\) (25%)

```python
from qiskit.quantum_info import Statevector
sv = Statevector([1/2, (3**0.5)/2])
print("Probabilities:", sv.probabilities())  # [0.25, 0.75]
```

</details>

<details>
<summary>Q2: Can you copy a qubit in superposition? (Hint: think about the no-cloning theorem)</summary>

**Answer:** No. The no-cloning theorem proves that arbitrary quantum states cannot be copied. This is a fundamental difference from classical bits.

</details>

<details>
<summary>Q3: What does measurement do to a superposition state?</summary>

**Answer:** It collapses the state to either |0⟩ or |1⟩, with probabilities determined by the amplitudes. The superposition is destroyed.

</details>

<details>
<summary>Q4: Is \\(e^{iπ}|ψ⟩\\) the same physical state as \\(|ψ⟩\\)?</summary>

**Answer:** Yes, up to global phase. Global phase changes are physically unobservable. But relative phase changes (like \\(e^{iπ}|1⟩\\)) are physically significant.

</details>

## Checkpoint Exercises

### Exercise 1

Use Qiskit to verify that \\(|+\\rangle\\) and \\(|-\\rangle\\) both have 50-50 measurement probabilities.

### Exercise 2

Create a state with P(|0⟩) = 0.8. What are the amplitudes?

### Exercise 3

Verify that \\(|+\\rangle\\) and \\(|-\\rangle\\) are orthogonal: \\(⟨+|−⟩ = 0\\).

### Exercise 4

Show that \\(|0⟩\\) and \\(|1⟩\\) are also orthogonal.

### Exercise 5

Explain in your own words why amplitudes must be normalized.

______________________________________________________________________

*Next: [Measurement: From Quantum to Classical](./measurement.md)*
