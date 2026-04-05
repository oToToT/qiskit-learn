# Qubit Ordering: The Endian Problem

This chapter exists because qubit ordering trips up nearly every quantum computing beginner. The good news: once you understand it, it becomes automatic.

## The Big Idea

**Qiskit uses little-endian ordering for basis states.**

This means:

- Qubit 0 is the **least significant bit** (rightmost)
- Qubit 1 is to the left of qubit 0
- Qubit 2 is further left, etc.

```
For a 3-qubit system with q2=1, q1=0, q0=1:
┌────────────────────────────────────────────┐
│  Basis label: |101⟩                        │
│  Qubit 2: 1  |  Qubit 1: 0  |  Qubit 0: 1  │
│      (MSB)                       (LSB)     │
└────────────────────────────────────────────┘
```

## Why This Matters

When you see a basis state like `|101>`:

- It means qubit 2 is 1
- It means qubit 1 is 0
- It means qubit 0 is 1

This is **backwards from how we normally write numbers** (where the leftmost digit is most significant).

## A Concrete Example

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Prepare |10⟩ on two qubits
qc = QuantumCircuit(2)
qc.x(1)  # Flip qubit 1

state = Statevector.from_instruction(qc)
print(state)
```

Output:

```
Statevector([0.+0.j, 0.+0.j, 0.+0.j, 1.+0.j],
            dims=(2, 2))
```

The statevector shows the amplitudes in order: `|00⟩`, `|01⟩`, `|10⟩`, `|11⟩`.

The `|10⟩` component has amplitude 1, meaning the state is \\(|10\\rangle\\).

## The Mapping

For 2 qubits, the statevector position maps to basis states like this:

| Position | Statevector Index | Basis State | Qubit Values |
|----------|-------------------|-------------|--------------|
| 0 | `[0]` | `\|00⟩` | q1=0, q0=0 |
| 1 | `[1]` | `\|01⟩` | q1=0, q0=1 |
| 2 | `[2]` | `\|10⟩` | q1=1, q0=0 |
| 3 | `[3]` | `\|11⟩` | q1=1, q0=1 |

## Preparing Specific States

To prepare `|101>` on 3 qubits:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)

# q0 = 1 (rightmost qubit)
qc.x(0)

# q2 = 1 (leftmost qubit)
qc.x(2)

state = Statevector.from_instruction(qc)
print(state)
```

Output shows `|101>` as the only non-zero amplitude.

## Measurement Output

The same ordering applies to measurement results:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(2)
qc.x(0)  # Set q0 = 1
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=100).result()
print(result[0].data.meas.get_counts())
```

Output:

```
{'01': 100}
```

The count shows `01` (not `10`) because q1=0 and q0=1.

## The Intuition

Think of it like reading a phone number:

```
555-1234
│   │
│   └── Least significant (rightmost)
└──────── Most significant (leftmost)
```

Qubits are numbered from right (least significant) to left (most significant).

## Why Little-Endian?

There's no deep quantum reason for this choice—it's a convention.

Some frameworks use big-endian. Qiskit chose little-endian because:

- It matches C/C++ integer representation
- It makes certain mathematical operations more natural
- It's consistent across IBM's quantum hardware

Once you know it's little-endian, you can work with any framework.

## A Common Mistake

Beginners often flip qubit indices when preparing states:

```python
# WRONG: Thinking q1=1 gives |01⟩
qc = QuantumCircuit(2)
qc.x(1)  # This actually gives |10⟩, not |01⟩!

# RIGHT: Check with Statevector
state = Statevector.from_instruction(qc)
print(state)
```

Always verify with `Statevector` until the ordering becomes second nature.

## A Systematic Approach

When you need to prepare a specific basis state:

1. **Write down the basis label**: `|101>`
1. **Break it into qubits**: q2=1, q1=0, q0=1
1. **Flip the right qubits**:
   ```python
   qc = QuantumCircuit(3)
   if q2 == 1: qc.x(2)  # Flip qubit 2
   if q1 == 1: qc.x(1)  # Flip qubit 1
   if q0 == 1: qc.x(0)  # Flip qubit 0
   ```
1. **Verify with Statevector**

## Tables to Memorize

### 2-Qubit Basis States

| Label | Qubit 1 | Qubit 0 | Statevector Index |
|-------|---------|---------|-------------------|
| `\|00⟩` | 0 | 0 | 0 |
| `\|01⟩` | 0 | 1 | 1 |
| `\|10⟩` | 1 | 0 | 2 |
| `\|11⟩` | 1 | 1 | 3 |

### 3-Qubit Basis States

| Label | Qubit 2 | Qubit 1 | Qubit 0 |
|-------|---------|---------|---------|
| `\|000⟩` | 0 | 0 | 0 |
| `\|001⟩` | 0 | 0 | 1 |
| `\|010⟩` | 0 | 1 | 0 |
| `\|011⟩` | 0 | 1 | 1 |
| `\|100⟩` | 1 | 0 | 0 |
| `\|101⟩` | 1 | 0 | 1 |
| `\|110⟩` | 1 | 1 | 0 |
| `\|111⟩` | 1 | 1 | 1 |

## General Rule

For an n-qubit system:

- Qubit 0 is always the rightmost (least significant)
- Qubit n-1 is always the leftmost (most significant)
- Basis state `|xₙ₋₁...x₂x₁x₀⟩` means qubit i has value xᵢ

## When Qubit Ordering Bites Back

These situations commonly cause confusion:

| Situation | What Goes Wrong |
|-----------|-----------------|
| Preparing arithmetic states | `|x+1⟩` often interpreted as different qubit ordering |
| Reading statevectors | Index doesn't match expected basis state |
| QCoder problems | Problem states might use different conventions |
| Copying circuit diagrams | Gate positions might assume different ordering |

## Debugging Tip

When a circuit "should" work but doesn't, qubit ordering is often the culprit.

Quick check:

```python
# Print both representations
qc = QuantumCircuit(2)
qc.x(1)

state = Statevector.from_instruction(qc)
print("State:", state)
print("Non-zero indices:", [i for i, amp in enumerate(state) if abs(amp) > 0])
```

If you expect `|01>` but get `|10>`, you found the bug.

## Summary

Key takeaways:

- Qiskit uses little-endian qubit ordering
- Qubit 0 is the least significant bit (rightmost)
- Basis state `|abc>` means q2=a, q1=b, q0=c (for 3 qubits)
- Always verify state preparations with `Statevector`
- Qubit ordering errors are the #1 source of beginner bugs

This convention will feel natural after some practice. When in doubt, check with `Statevector`.

______________________________________________________________________

## Checkpoint Exercises

1. Prepare `|10>` on two qubits and verify the statevector.
1. Prepare `|101>` on three qubits.
1. Explain why `qc.x(0)` on two qubits gives `|01>`.
1. Create a table showing all 2-qubit basis states with their qubit values.
1. Predict what `qc.x(0); qc.x(2)` produces on a 3-qubit system.

> **Official Documentation:** For more on qubit ordering and statevector representation, see the [Qiskit documentation on Statevector](https://docs.quantum.ibm.com/api/qiskit/qiskit.quantum_info.Statevector). The [QuantumCircuit qubit layout](https://docs.quantum.ibm.com/build/noise-models#configuring-circuit-run-time) documentation explains how qubit ordering affects circuit execution.

## Try These On QCoder

- [QPC001 A4: Generate State (|10> + |11>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC001/problems/A4)
- [QPC003 A2: Generate State (|10> + |01>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC003/problems/A2)
- [QPC003 A3: Generate State (|100> + |010> + |001>)/sqrt(3)](https://www.qcoder.jp/en/contests/QPC003/problems/A3)

______________________________________________________________________

*Next: [Gate Basics: X, Y, Z](./single-qubit-gates-xyz.md)*
