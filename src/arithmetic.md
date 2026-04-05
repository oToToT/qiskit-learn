# Arithmetic Circuits and Modular Thinking

Many quantum algorithms involve arithmetic operations. This chapter teaches you to think about quantum arithmetic as reversible basis-state transformations.

## The Reversibility Constraint

Classical arithmetic often loses information:

- AND: 2 inputs → 1 output
- Addition can overflow

Quantum arithmetic must be reversible:

- Use ancilla qubits for results
- Preserve inputs (or uncompute them)

## Basis State Mapping

Design arithmetic circuits by thinking about basis states:

| Operation | Mapping |
|-----------|---------|
| SWAP | \\(\|xy\\rangle \\rightarrow \|yx\\rangle\\) |
| Increment | \\(\|x\\rangle \\rightarrow \|x+1 \\mod 2^n\\rangle\\) |
| Add constant | \\(\|x\\rangle \\rightarrow \|x+k \\mod 2^n\\rangle\\) |

## SWAP: The Simplest Arithmetic Circuit

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# SWAP |xy⟩ → |yx⟩
qc = QuantumCircuit(2)
qc.swap(0, 1)

# Test on all inputs
for bits in ['00', '01', '10', '11']:
    qc_test = QuantumCircuit(2)
    for i, b in enumerate(reversed(bits)):
        if b == '1':
            qc_test.x(i)
    qc_test.compose(qc, inplace=True)
    print(f"{bits} → {Statevector.from_instruction(qc_test)}")
```

## Increment

Adding 1 modulo \\(2^n\\):

```python
def increment(n):
    """Create a circuit that adds 1 modulo 2^n."""
    qc = QuantumCircuit(n)
    for i in range(n):
        qc.x(i)  # Flip all qubits
    qc.x(0)  # Correct for the all-ones case
    return qc

# Test on 2 qubits
inc = increment(2)
for bits in ['00', '01', '10', '11']:
    qc_test = QuantumCircuit(2)
    for i, b in enumerate(reversed(bits)):
        if b == '1':
            qc_test.x(i)
    qc_test.compose(inc, inplace=True)
    print(f"{bits}+1 = {Statevector.from_instruction(qc_test)}")
```

## Addition in the Fourier Basis

Addition becomes phase rotation in the Fourier basis:

```python
# Add k in Fourier basis
def add_fourier(k, n):
    qc = QuantumCircuit(n)
    for i in range(n):
        qc.p(2*pi*k/(2**(i+1)), i)
    return qc
```

This is more efficient than computational basis addition.

## Modular Arithmetic

Many quantum algorithms require modular arithmetic (operations modulo \\(N\\)):

\\[x + y \\pmod{N}\\]

Key property: all operations wrap around at \\(N\\).

```python
# Modular addition: (x + k) mod 4
def add_mod_4(k):
    qc = QuantumCircuit(2)
    for _ in range(k):
        qc.x(0)
        qc.x(1)
        qc.x(0)  # Correct for overflow
    return qc
```

## Design Patterns

### Pattern 1: Start with Basis States

Before writing gates, write the intended mapping:

```
|w⟩ → |w⟩    (identity)
|w⟩ → |f(w)⟩ (transformation)
```

### Pattern 2: Test Exhausively

For small circuits, test all possible inputs:

```python
def test_circuit(circuit, n_qubits, expected_mapping):
    for i in range(2**n_qubits):
        qc = QuantumCircuit(n_qubits)
        # Prepare basis state |i⟩
        for j in range(n_qubits):
            if (i >> j) & 1:
                qc.x(j)
        qc.compose(circuit, inplace=True)
        state = Statevector.from_instruction(qc)
        # Check result
```

### Pattern 3: Use Ancillas Wisely

- Use ancillas for intermediate results
- Uncompute to restore ancillas to |0⟩
- Minimize ancilla count when possible

## Arithmetic and QFT

Some arithmetic is easier in the Fourier basis:

- Phase rotations implement addition
- QFT → add phases → inverse QFT

This is how Shor's algorithm implements modular exponentiation.

## Checkpoint Exercises

### Exercise 1

Implement a 2-qubit decrement (subtract 1).

### Exercise 2

Implement a 2-qubit circuit that computes \\(|x\\rangle \\rightarrow |2x \\mod 4\\rangle\\).

### Exercise 3

Test your increment circuit exhaustively on all 2-bit inputs.

### Exercise 4

Design a circuit that swaps \\(|01\\rangle\\) and \\(|10\\rangle\\) but leaves \\(|00\\rangle\\) and \\(|11\\rangle\\) unchanged.

### Exercise 5

Explain why modular arithmetic is necessary for quantum algorithms.

## Summary

Key concepts:

- Arithmetic circuits map basis states
- Must be reversible (use ancillas)
- Test exhaustively on small inputs
- Fourier basis simplifies some operations
- Modular arithmetic wraps around

Arithmetic circuits are building blocks for quantum algorithms like Shor's.

______________________________________________________________________

## Try These On QCoder

- [QPC002 B3: SWAP Qubits](https://www.qcoder.jp/en/contests/QPC002/problems/B3)
- [QPC002 B5-B8](https://www.qcoder.jp/en/contests/QPC002)
- [QPC005 B2: Cyclic Shift](https://www.qcoder.jp/en/contests/QPC005/problems/B2)

______________________________________________________________________

*Next: [A Productive Qiskit Workflow](./qiskit-workflow.md)*
