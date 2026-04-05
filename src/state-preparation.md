# State Preparation Patterns

State preparation is the art of designing circuits that produce specific quantum states. This chapter covers the most common patterns you'll encounter.

## Pattern 1: Basis States

The simplest pattern: flip qubits to match the target basis state.

To prepare \\(|101\\rangle\\):

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)
qc.x(0)  # q0 = 1 (rightmost)
qc.x(2)  # q2 = 1 (leftmost)
print(Statevector.from_instruction(qc))
```

Remember: qubit 0 is the rightmost (least significant).

## Pattern 2: Uniform Superposition

Apply H to all qubits:

```python
def uniform_superposition(n):
    qc = QuantumCircuit(n)
    for i in range(n):
        qc.h(i)
    return qc

# Test on 4 qubits
qc = uniform_superposition(4)
state = Statevector.from_instruction(qc)
print(state)
```

This creates \\(\\frac{1}{\\sqrt{2^n}}\\sum\_{x=0}^{2^n-1}|x\\rangle\\).

## Pattern 3: Superposition with Specific Weights

Use RY to control probabilities:

```python
from math import pi, asin, sqrt

def prepare_amplitudes(amplitudes):
    """Prepare a 1-qubit state with given amplitudes."""
    # amplitudes = [alpha, beta]
    alpha, beta = amplitudes
    theta = 2 * asin(abs(beta))
    qc = QuantumCircuit(1)
    qc.ry(theta, 0)
    if beta.real < 0:
        qc.z(0)
    return qc

# Create (√(3)/2)|0⟩ + (1/2)|1⟩
qc = prepare_amplitudes([sqrt(3)/2, 0.5])
print(Statevector.from_instruction(qc))
```

## Pattern 4: Two-Qubit Routing

For states like \\(\\sqrt{\\frac{3}{4}}|00\\rangle + \\frac{1}{2}|11\\rangle\\):

```python
from math import pi

qc = QuantumCircuit(2)
# Use RY to set the branch split
qc.ry(pi/3, 0)  # cos(π/6)² = 3/4, sin(π/6)² = 1/4
# Copy the branch to qubit 1
qc.cx(0, 1)
print(Statevector.from_instruction(qc))
```

The RY sets probabilities, then CNOT routes them.

## Pattern 5: Unequal Superposition

Create superpositions over a subset of states:

```python
# (|10⟩ + |11⟩)/√2
qc = QuantumCircuit(2)
qc.h(0)  # Creates (|0⟩ + |1⟩)/√2 on qubit 0
qc.x(1)  # Set qubit 1 to |1⟩
# Result: (|01⟩ + |11⟩)/√2? No! Check with statevector
print(Statevector.from_instruction(qc))
```

Try this and verify. Then figure out how to get \\((|10\\rangle + |11\\rangle)/\\sqrt{2}\\).

## Pattern 6: Sign-Encoded Information

Add phases to distinguish states:

```python
# (|00⟩ - |11⟩)/√2
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.z(0)
print(Statevector.from_instruction(qc))
```

Sign differences are invisible in immediate measurement but visible in interference.

## Pattern 7: Multi-Qubit GHZ and W States

### GHZ State

```python
def ghz_state(n):
    qc = QuantumCircuit(n)
    qc.h(0)
    for i in range(n-1):
        qc.cx(i, i+1)
    return qc

qc = ghz_state(5)
print(Statevector.from_instruction(qc))
```

Creates \\(\\frac{|0...0\\rangle + |1...1\\rangle}{\\sqrt{2}}\\).

### W State

```python
def w_state(n):
    """Create (|100...0⟩ + |010...0⟩ + ... + |000...1⟩)/√n"""
    qc = QuantumCircuit(n)
    # More complex—requires multi-controlled rotations
    # We'll cover this later
    return qc
```

The W state is more complex to construct. For now, trust that it exists.

## The State Preparation Checklist

When designing a state preparation circuit:

1. **Name the target state** in mathematical notation
1. **Identify probabilities** for each basis state
1. **Use RY to set branch probabilities**
1. **Use CNOT to route amplitude** to correct basis states
1. **Add phases** with Z or CZ to distinguish states
1. **Verify** with Statevector before proceeding

## Manual vs Convenience Methods

Qiskit has `QuantumCircuit.initialize()`:

```python
from math import sqrt

qc = QuantumCircuit(2)
qc.initialize([sqrt(3)/2, 0, 0, 0.5])
print(Statevector.from_instruction(qc))
```

This works but hides the circuit structure. **Manual construction teaches more.**

## Checkpoint Exercises

### Exercise 1

Prepare \\(\\frac{|00\\rangle + |01\\rangle + |10\\rangle}{\\sqrt{3}}\\) (3-way superposition on 2 qubits)

### Exercise 2

Prepare \\(|010\\rangle\\) on 3 qubits

### Exercise 3

Prepare \\(\\frac{|00\\rangle + |11\\rangle}{\\sqrt{2}}\\)

### Exercise 4

Prepare \\(\\frac{|100\\rangle + |010\\rangle + |001\\rangle}{\\sqrt{3}}\\) (W state)

### Exercise 5

Design a state preparation for \\((|000\\rangle + |111\\rangle)/\\sqrt{2}\\)

## Summary

Key patterns:

- Basis states: flip qubits with X
- Superposition: apply H gates
- Unequal amplitudes: use RY
- Routing: use CNOT
- Phases: use Z/CZ

State preparation is foundational for everything that follows.

______________________________________________________________________

## Try These On QCoder

- [QPC001 A5: Generate State (sqrt(3)/2)|100> + (1/2)|011>](https://www.qcoder.jp/en/contests/QPC001/problems/A5)
- [QPC003 A3: Generate State (|100> + |010> + |001>)/sqrt(3)](https://www.qcoder.jp/en/contests/QPC003/problems/A3)
- [QPC003 A4, A5, A6](https://www.qcoder.jp/en/contests/QPC003)

______________________________________________________________________

*Next: [Phase Kickback: Encoding in Phase](./phase-kickback.md)*
