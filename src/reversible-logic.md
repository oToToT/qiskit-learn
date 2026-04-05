# Reversible Logic and Oracles

Quantum circuits must be **reversible**—you can always run them backward. This chapter teaches you to think about quantum computation as reversible transformation.

## Why Reversibility Matters

Classical computing:

- AND: 2 inputs → 1 output (information loss)
- NOT: 1 input → 1 output (reversible)

Quantum computing:

- All gates must be unitary (reversible)
- Uncomputing: computing a result, using it, then uncomputing

## Bit-Flip Oracles

A bit-flip oracle flips an ancilla qubit when a condition is met:

\\[|x\\rangle|b\\rangle \\rightarrow |x\\rangle|b \\oplus f(x)\\rangle\\]

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Oracle that flips ancilla when x = 11
def oracle_11():
    qc = QuantumCircuit(3)  # x[0], x[1], ancilla
    qc.ccx(0, 1, 2)  # Toffoli: flip ancilla if x0=1 AND x1=1
    return qc

# Test on |11⟩
qc = QuantumCircuit(3)
qc.x([0, 1])  # |11⟩
qc.x(2)        # ancilla = |1⟩
qc.compose(oracle_11(), inplace=True)
print(Statevector.from_instruction(qc))
```

The ancilla flips from \\(|1\\rangle\\) to \\(|0\\rangle\\).

## Phase Oracles

A phase oracle marks states with a phase:

\\[|x\\rangle \\rightarrow (-1)^{f(x)}|x\\rangle\\]

```python
# Phase oracle for |11⟩
def phase_oracle_11():
    qc = QuantumCircuit(2)
    qc.cz(0, 1)  # Flip phase of |11⟩
    return qc
```

Phase oracles are simpler because they don't need an ancilla.

## Converting Bit-Flip to Phase

Use the \\(|-\\rangle\\) ancilla trick:

```python
# Convert bit-flip oracle to phase oracle
def bitflip_to_phase(oracle):
    """Wrap a bit-flip oracle with |-⟩ ancilla."""
    qc = QuantumCircuit(oracle.num_qubits + 1)
    
    # Prepare |-⟩ on ancilla (last qubit)
    qc.x(oracle.num_qubits)
    qc.h(oracle.num_qubits)
    
    # Apply oracle
    qc.compose(oracle, inplace=True)
    
    # Uncompute |-⟩ preparation
    qc.h(oracle.num_qubits)
    qc.x(oracle.num_qubits)
    
    return qc

# Test
original = oracle_11()
converted = bitflip_to_phase(original)
print(Operator(original))
print(Operator(converted))
```

## Reversible Arithmetic

Classical operations can be made reversible:

```python
# Reversible XOR (CNOT)
qc = QuantumCircuit(2)
qc.cx(0, 1)  # q1 = q1 XOR q0
# To reverse: apply again
qc.cx(0, 1)  # Back to original
```

```python
# Reversible AND (Toffoli/CCX)
qc = QuantumCircuit(3)
qc.ccx(0, 1, 2)  # q2 = q2 AND q1 AND q0
# Toffoli is self-inverse
qc.ccx(0, 1, 2)  # Back to original
```

## The Uncomputation Pattern

1. Compute auxiliary values
1. Use the values
1. **Uncompute** to restore them

```python
# Compute f(x), use it, uncompute
def use_reversibly():
    qc = QuantumCircuit(4)
    
    # Compute: ancilla = f(x)
    qc.ccx(0, 1, 3)  # ancilla = x0 AND x1
    
    # Use ancilla (e.g., controlled operation)
    qc.ccx(3, 2, 3)  # something using ancilla
    
    # Uncompute: restore ancilla to |0⟩
    qc.ccx(0, 1, 3)  # Uncompute
    
    return qc
```

## Uncomputing Is Not Optional

If you don't uncompute:

- Work qubits stay entangled with data
- Noise accumulates
- Algorithm correctness fails

Always uncompute when done!

## Checkpoint Exercises

### Exercise 1

Build a phase oracle that marks \\(|01\\rangle\\).

### Exercise 2

Build a phase oracle that marks \\(|100\\rangle\\).

### Exercise 3

Convert a bit-flip oracle for \\(|11\\rangle\\) to a phase oracle.

### Exercise 4

Implement a reversible XOR circuit and verify it uncomputes correctly.

### Exercise 5

Design a reversible circuit that computes \\(|x, y\\rangle \\rightarrow |x, y \\oplus x\\rangle\\).

## Summary

Key concepts:

- Bit-flip oracles: flip ancilla based on input
- Phase oracles: mark states with phase
- Conversion via \\(|-\\rangle\\) ancilla
- All quantum operations are reversible
- Uncompute auxiliary qubits after use

Oracles are the building blocks of Grover's algorithm.

______________________________________________________________________

## Try These On QCoder

- [QPC002 B2: Phase Shift Oracle](https://www.qcoder.jp/en/contests/QPC002/problems/B2)
- [QPC003 B2: Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1: Convert Bit-Flip into Phase-Flip II](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)

______________________________________________________________________

*Next: [Reflections and Amplitude Amplification](./reflections.md)*
