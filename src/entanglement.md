# Entanglement and Bell States

Entanglement is perhaps the most distinctly quantum phenomenon. It's what makes quantum computers fundamentally different from classical computers.

## What Is Entanglement?

Two qubits are **entangled** if their joint state cannot be written as a product of individual states.

**Separable (not entangled):**
\\[|\\psi\\rangle = |\\phi\\rangle \\otimes |\\chi\\rangle\\]

**Entangled:**
\\[|\\psi\\rangle \\neq |\\phi\\rangle \\otimes |\\chi\\rangle\\]

Example of entangled state:
\\[\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}} \\neq \\left(\\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\right) \\otimes \\left(\\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\right)\\]

## The Four Bell States

The Bell states are the four maximal entangled two-qubit states:

| Name | Definition | Created By |
|------|------------|------------|
| \\(|\\Phi^+\\rangle\\) | \\(\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}}\\) | \\(H_0\\), CNOT |
| \\(|\\Phi^-\\rangle\\) | \\(\\frac{|00\\rangle - |11\\rangle}{\\sqrt{2}}\\) | \\(H_0\\), CNOT, \\(Z_0\\) |
| \\(|\\Psi^+\\rangle\\) | \\(\\frac{|01\\rangle + |10\\rangle}{\\sqrt{2}}\\) | \\(X_0\\), \\(H_0\\), CNOT |
| \\(|\\Psi^-\\rangle\\) | \\(\\frac{|01\\rangle - |10\\rangle}{\\sqrt{2}}\\) | \\(X_0\\), \\(H_0\\), CNOT, \\(Z_0\\) |

## Creating the Four Bell States

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def bell_state(variant='+'):
    """Create one of the four Bell states."""
    qc = QuantumCircuit(2)
    
    if variant in ['Φ⁺', 'phi+', '+', 0]:
        # |Φ⁺⟩ = (|00⟩ + |11⟩) / √2
        qc.h(0)
        qc.cx(0, 1)
        
    elif variant in ['Φ⁻', 'phi-', '-', 1]:
        # |Φ⁻⟩ = (|00⟩ - |11⟩) / √2
        qc.h(0)
        qc.cx(0, 1)
        qc.z(0)
        
    elif variant in ['Ψ⁺', 'psi+', 2]:
        # |Ψ⁺⟩ = (|01⟩ + |10⟩) / √2
        qc.x(0)
        qc.h(0)
        qc.cx(0, 1)
        
    elif variant in ['Ψ⁻', 'psi-', 3]:
        # |Ψ⁻⟩ = (|01⟩ - |10⟩) / √2
        qc.x(0)
        qc.h(0)
        qc.cx(0, 1)
        qc.z(0)
    
    return qc

# Create and display each Bell state
for variant in ['Φ⁺', 'Φ⁻', 'Ψ⁺', 'Ψ⁻']:
    qc = bell_state(variant)
    state = Statevector.from_instruction(qc)
    print(f"{variant}: {state}")
```

## Visualizing Entangled States

Unlike single-qubit states, entangled states cannot be visualized on individual Bloch spheres—each qubit's state depends on the other. However, we can still visualize the individual qubit states to understand the correlations:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# |Φ⁺⟩ = (|00⟩ + |11⟩)/√2 - correlated states
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
state = Statevector.from_instruction(qc)
print("|Φ⁺⟩ state:")
print(state)
display(plot_bloch_multivector(state))

# |Ψ⁺⟩ = (|01⟩ + |10⟩)/√2 - anticorrelated
qc = QuantumCircuit(2)
qc.x(0)
qc.h(0)
qc.cx(0, 1)
state = Statevector.from_instruction(qc)
print("|Ψ⁺⟩ state:")
print(state)
display(plot_bloch_multivector(state))
```

Notice that when you measure one qubit of an entangled state, the other qubit's state is immediately determined!

## What Makes Entanglement Special?

### 1. Perfect Correlation

If you measure one qubit of \\(|\\Phi^+\\rangle\\), you know the other instantly:

```
Measure qubit 0 → |0⟩  ⇒  qubit 1 is |0⟩
Measure qubit 0 → |1⟩  ⇒  qubit 1 is |1⟩
```

This is true *regardless of distance* between the qubits.

### 2. No Local Hidden Variables

Classical intuition says: the correlation must come from:

- Pre-determined outcomes, or
- Hidden signals between particles

Quantum mechanics says: neither is true. The outcomes aren't determined until measurement.

### 3. Cannot Be Copied

You cannot clone an entangled state:
\\[|\\psi\\rangle \\otimes |\\phi\\rangle \\neq |\\psi\\rangle \\otimes |\\psi\\rangle\\]

This is the **no-cloning theorem**—a fundamental consequence of quantum mechanics.

## Measuring Bell States

Let's see the correlation in action:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

# Create |Φ⁺⟩
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

# Sample many times
sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()
print(result[0].data.meas.get_counts())
```

Typical output (always the same pattern):

```
{'00': 498, '11': 502}
```

Only `00` and `11` appear. Never `01` or `10`.

## Changing the Correlation

Different Bell states give different correlations:

```python
# |Φ⁻⟩ = (|00⟩ - |11⟩) / √2
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.z(0)  # This flips the phase
qc.measure_all()

result = sampler.run([qc], shots=1000).result()
print("|Φ⁻⟩:", result[0].data.meas.get_counts())
```

The counts are the same (50-50 on `00` and `11`), but the phase is different.

## Verifying Entanglement

How do you know if a state is entangled? One test: try to write it as a product state.

```python
from qiskit.quantum_info import Statevector

# This IS separable: (|0⟩ + |1⟩)/√2 ⊗ (|0⟩ + |1⟩)/√2
state1 = Statevector.from_instruction(QuantumCircuit(2))
print("Separable:", state1)

# This is NOT separable: |Φ⁺⟩
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
state2 = Statevector.from_instruction(qc)
print("Entangled:", state2)
```

For two qubits, you can check if a state is entangled by looking at the reduced density matrix—but that's a topic for later.

## Entanglement in Multi-Qubit Systems

### GHZ State (3 qubits)

\\[\\frac{|000\\rangle + |111\\rangle}{\\sqrt{2}}\\]

```python
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(1, 2)
print(Statevector.from_instruction(qc))
```

### W State (3 qubits)

\\[\\frac{|100\\rangle + |010\\rangle + |001\\rangle}{\\sqrt{3}}\\]

This is a different kind of entanglement—all three qubits are involved, but no pair alone determines the third.

## Why Entanglement Matters

Entanglement is the resource for:

1. **Quantum teleportation** — transferring quantum states
1. **Superdense coding** — sending 2 classical bits with 1 quantum bit
1. **Quantum key distribution** — secure communication
1. **Quantum algorithms** — speedups in computation

Without entanglement, quantum computers would be no more powerful than classical computers.

## Common Misconceptions

| Misconception | Reality |
|---------------|---------|
| "Entanglement means information travels instantly" | No information is transferred. Measurement outcomes are random. |
| "Entanglement is like correlation in classical systems" | Classical correlation can't explain quantum violations of Bell inequalities |
| "Entanglement proves quantum weirdness" | Entanglement is a consequence of superposition + multi-qubit gates |

## Checkpoint Exercises

### Exercise 1

Create all four Bell states and verify their statevectors.

### Exercise 2

Measure a Bell state 1000 times. What patterns do you see?

### Exercise 3

Create the GHZ state and measure it. What are the possible outcomes?

### Exercise 4

Explain why \\(|\\Phi^+\\rangle\\) cannot be written as a product of two single-qubit states.

### Exercise 5

Create the W state \\(\\frac{|100\\rangle + |010\\rangle + |001\\rangle}{\\sqrt{3}}\\) on three qubits.

## Summary

Key concepts:

- Entanglement: states that cannot be written as products
- Four Bell states: \\(|\\Phi^\\pm\\rangle\\) and \\(|\\Psi^\\pm\\rangle\\)
- Perfect correlations in measurement outcomes
- Cannot be explained by classical physics
- No cloning possible
- Resource for quantum communication and algorithms

Entanglement is what makes quantum computing special. Understanding Bell states prepares you for the circuit patterns that power quantum algorithms.

> **Official Documentation:** For more on entanglement and Bell states, see IBM's [entanglement tutorial](https://learning.quantum.ibm.com/tutorial/entanglement). The [Qiskit BellState circuit](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.BellGate) provides a convenient way to create Bell states.

______________________________________________________________________

## Try These On QCoder

- [QPC001 A4: Generate State (|10> + |11>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC001/problems/A4)
- [QPC003 A2: Generate State (|10> + |01>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC003/problems/A2)
- [QPC003 A3: Generate State (|100> + |010> + |001>)/sqrt(3)](https://www.qcoder.jp/en/contests/QPC003/problems/A3)

______________________________________________________________________

*Next: [More Two-Qubit Gates: CZ, SWAP](./two-qubit-more.md)*
