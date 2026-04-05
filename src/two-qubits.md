# Controlled Operations: CNOT

This is where quantum computing gets interesting. The CNOT (Controlled-NOT) gate is your first two-qubit gate—it creates correlations between qubits that have no classical analog.

## The CNOT Gate

The CNOT (Controlled-X) gate has two qubits:

- **Control qubit**: The one that decides whether to act
- **Target qubit**: The one that gets flipped

```
CNOT:  ┌───┐
control:─┤   ├──
         │ X │
target:──┤   ├──
         └───┘
```

The rule: \*\*flip the target if and only if the control is \\(|1\\rangle\\).

## CNOT on Basis States

Let's see what happens:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Case 1: Control = 0, Target = 0 → No change
qc1 = QuantumCircuit(2)
qc1.cx(0, 1)  # CNOT with control=0, target=1
print("|00⟩ after CNOT:", Statevector.from_instruction(qc1))

# Case 2: Control = 1, Target = 0 → Flip target
qc2 = QuantumCircuit(2)
qc2.x(0)  # Set control to 1
qc2.cx(0, 1)
print("|10⟩ after CNOT:", Statevector.from_instruction(qc2))

# Case 3: Control = 0, Target = 1 → No change
qc3 = QuantumCircuit(2)
qc3.x(1)  # Set target to 1
qc3.cx(0, 1)
print("|01⟩ after CNOT:", Statevector.from_instruction(qc3))

# Case 4: Control = 1, Target = 1 → Flip target
qc4 = QuantumCircuit(2)
qc4.x([0, 1])  # Both are 1
qc4.cx(0, 1)
print("|11⟩ after CNOT:", Statevector.from_instruction(qc4))
```

Summary:
| Input | Output |
|-------|--------|
| `\|00⟩` | `\|00⟩` |
| `\|01⟩` | `\|01⟩` |
| `\|10⟩` | `\|11⟩` |
| `\|11⟩` | `\|10⟩` |

## CNOT Syntax in Qiskit

```python
qc.cx(control, target)
```

The first argument is the control qubit, the second is the target.

```python
qc.cx(0, 1)  # Qubit 0 is control, qubit 1 is target
qc.cx(1, 0)  # Qubit 1 is control, qubit 0 is target
```

**These are NOT the same!** The control-target orientation matters.

## CNOT on Superpositions

Here's where it gets interesting:

```python
# CNOT on superposition state
qc = QuantumCircuit(2)
qc.h(0)  # Put control in superposition
qc.cx(0, 1)

print("State:", Statevector.from_instruction(qc))
```

Output:

```
Statevector([0.70710678+0.j, 0.+0.j, 0.+0.j, 0.70710678+0.j],
            dims=(2, 2))
```

This is \\(\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}}\\)—a **Bell state**!

## Reading the Statevector

For a 2-qubit statevector, the indices correspond to basis states:

| Index | Basis State | Value |
|-------|-------------|-------|
| 0 | `\|00⟩` | 0.707... |
| 1 | `\|01⟩` | 0 |
| 2 | `\|10⟩` | 0 |
| 3 | `\|11⟩` | 0.707... |

The state is \\(\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}}\\).

## Why This Is Special

The state \\(\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}}\\) has a property that classical bits can't replicate: **quantum correlation**.

If you measure qubit 0:

- You see `0` with 50% probability → qubit 1 is also `0`
- You see `1` with 50% probability → qubit 1 is also `1`

The outcomes are **perfectly correlated**, even though neither outcome is predetermined!

## CNOT Is Not Reversible? Wrong!

Classical XOR is not reversible: `0 XOR 0 = 0`, but so does `1 XOR 1 = 0`.

Quantum CNOT **is** reversible:

- Apply CNOT twice and you get back to the original state
- The quantum operation is unitary

```python
# CNOT twice = identity
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 1)

print("CNOT twice:", Statevector.from_instruction(qc))
```

## Circuit Diagrams

Qiskit prints readable circuit diagrams:

```python
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
print(qc)
```

```
     ┌───┐
q_0: ┤ H ├──■──
     └───┘  │
q_1: ──────■──
```

The `■` is the control, the `⊕` is the target (often shown as `■───⊕───`).

## More Complex CNOT Patterns

### CNOT Chain

```python
# Create a GHZ state (Greenberger-Horne-Zeilinger)
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(1, 2)
print(Statevector.from_instruction(qc))
```

This creates \\(\\frac{|000\\rangle + |111\\rangle}{\\sqrt{2}}\\).

### CNOT Fan-Out

```python
# One control, multiple targets
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)
print(Statevector.from_instruction(qc))
```

This creates \\(\\frac{|000\\rangle + |110\\rangle}{\\sqrt{2}}\\)? No wait—verify this yourself!

## Checkpoint Exercises

### Exercise 1

Apply CNOT to \\(|01\\rangle\\). What state do you get?

### Exercise 2

Apply CNOT to \\(|10\\rangle\\). What state do you get?

### Exercise 3

Create the Bell state \\(\\frac{|01\\rangle + |10\\rangle}{\\sqrt{2}}\\). (Hint: start with the right basis state)

### Exercise 4

Create the GHZ state \\(\\frac{|000\\rangle + |111\\rangle}{\\sqrt{2}}\\) on three qubits.

### Exercise 5

Show that CNOT is its own inverse (applying it twice returns to the original state).

## Common Mistakes

| Mistake | Result |
|---------|--------|
| `cx(1, 0)` instead of `cx(0, 1)` | Wrong correlation direction |
| Forgetting to put control in superposition | No entanglement, just classical correlation |
| Measuring before CNOT | Destroys superposition before entanglement forms |

## Summary

Key concepts:

- CNOT flips the target if control is \\(|1\\rangle\\)
- Syntax: `cx(control, target)` in Qiskit
- On superposition: creates Bell states
- CNOT is reversible (its own inverse)
- Creates quantum correlations (entanglement)

The CNOT gate is the workhorse of two-qubit quantum circuits. Master it, and everything else becomes easier.

> **Official Documentation:** The CNOT (Controlled-X) gate is documented in the [Qiskit circuit library](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.CXGate). See also IBM's tutorial on [multi-qubit circuits](https://learning.quantum.ibm.com/tutorial/multi-qubit-circuits).

______________________________________________________________________

*Next: [Entanglement and Bell States](./entanglement.md)*
