# Grover Search

Grover's algorithm is the most famous quantum search algorithm. It finds a marked item in an unsorted database of \\(N\\) items in \\(O(\\sqrt{N})\\) steps— quadratically faster than classical search.

## The Problem

Given an unsorted list of \\(N\\) items, find the one marked "good."

Classically: \\(O(N)\\) operations.

With Grover: \\(O(\\sqrt{N})\\) operations.

## The Quantum Insight

Rather than searching through items one by one, Grover's algorithm:

1. Creates uniform superposition over all items
1. Uses an oracle to mark the target
1. Amplifies the marked state's amplitude (amplitude amplification)

## The Circuit Structure

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  H⊗n ──[Oracle]── H⊗n ──[Diffusion]── H⊗n ──[Oracle]── ...│
│                                                             │
│              One Grover Iteration                           │
└─────────────────────────────────────────────────────────────┘
```

## Building Grover's Algorithm

### Step 1: Uniform Superposition

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from math import pi

n = 2  # 2 qubits = 4 items
qc = QuantumCircuit(n)
qc.h(range(n))
print("Uniform superposition:", Statevector.from_instruction(qc))
```

### Step 2: Oracle (Mark the Target)

For target \\(|11\\rangle\\):

```python
def oracle_11():
    qc = QuantumCircuit(n)
    qc.cz(0, 1)  # Flip phase of |11⟩
    return qc

qc.compose(oracle_11(), inplace=True)
print("After oracle:", Statevector.from_instruction(qc))
```

### Step 3: Diffusion (Amplify)

```python
def diffusion():
    qc = QuantumCircuit(n)
    qc.h(range(n))
    qc.x(range(n))
    qc.h(n-1)
    qc.mcx(list(range(n-1)), n-1)
    qc.h(n-1)
    qc.x(range(n))
    qc.h(range(n))
    return qc

qc.compose(diffusion(), inplace=True)
print("After diffusion:", Statevector.from_instruction(qc))
```

### Step 4: Measure

```python
qc.measure_all()
from qiskit.primitives import StatevectorSampler
sampler = StatevectorSampler()
result = sampler.run([qc], shots=256).result()
print(result[0].data.meas.get_counts())
```

The target \\(|11\\rangle\\) should dominate.

## Understanding the Amplification

| Stage | \\(\|11\\rangle\\) Amplitude |
|-------|------------------------|
| Start (uniform) | \\(1/\\sqrt{4} = 0.5\\) |
| After oracle | \\(-0.5\\) (flipped sign) |
| After diffusion | \\(\\approx 1.0\\) (amplified) |

The diffusion step reflects about the mean amplitude, boosting the negative amplitude.

## Optimal Iteration Count

For \\(N\\) items with \\(M\\) marked items:

- Optimal iterations \\(\\approx \\frac{\\pi}{4}\\sqrt{\\frac{N}{M}}\\)
- Too few: amplitude not fully amplified
- Too many: amplitude rotates past target

```python
def grover_search(n, target, iterations=1):
    qc = QuantumCircuit(n)
    qc.h(range(n))
    
    for _ in range(iterations):
        # Oracle
        for i, bit in enumerate(target):
            if bit == '1':
                qc.x(i)
        qc.h(n-1)
        qc.mcx(list(range(n-1)), n-1)
        qc.h(n-1)
        for i, bit in enumerate(target):
            if bit == '1':
                qc.x(i)
        
        # Diffusion
        qc.h(range(n))
        qc.x(range(n))
        qc.h(n-1)
        qc.mcx(list(range(n-1)), n-1)
        qc.h(n-1)
        qc.x(range(n))
        qc.h(range(n))
    
    qc.measure_all()
    return qc
```

## Testing on Different Targets

```python
# Search for |01⟩ on 2 qubits
qc = grover_search(2, '01', iterations=1)
result = sampler.run([qc], shots=256).result()
print("Searching for |01⟩:", result[0].data.meas.get_counts())
```

## Visualizing Amplitude Evolution

```python
import matplotlib.pyplot as plt

def grover_amplitudes(n, target):
    """Return amplitudes after each iteration."""
    amplitudes = []
    
    qc = QuantumCircuit(n)
    qc.h(range(n))
    amplitudes.append(Statevector.from_instruction(qc))
    
    for _ in range(3):  # 3 iterations
        # Oracle
        for i, bit in enumerate(target):
            if bit == '1':
                qc.x(i)
        qc.h(n-1)
        qc.mcx(list(range(n-1)), n-1)
        qc.h(n-1)
        for i, bit in enumerate(target):
            if bit == '1':
                qc.x(i)
        
        # Diffusion
        qc.h(range(n))
        qc.x(range(n))
        qc.h(n-1)
        qc.mcx(list(range(n-1)), n-1)
        qc.h(n-1)
        qc.x(range(n))
        qc.h(range(n))
        
        amplitudes.append(Statevector.from_instruction(qc))
    
    return amplitudes

# Plot amplitudes for |11⟩ on 2 qubits
amplitudes = grover_amplitudes(2, '11')
for i, amp in enumerate(amplitudes):
    print(f"Iteration {i}: |11⟩ amplitude = {amp[3]:.3f}")
```

## Checkpoint Exercises

### Exercise 1

Implement Grover's search for \\(|00\\rangle\\) on 2 qubits.

### Exercise 2

Search for \\(|101\\rangle\\) on 3 qubits.

### Exercise 3

Compare 1, 2, and 3 iterations. What happens with too many?

### Exercise 4

Search for one marked item among 8 (3 qubits). Verify speedup.

### Exercise 5

Explain why Grover's algorithm is optimal.

## Summary

Key concepts:

- Grover searches unsorted data in \\(O(\\sqrt{N})\\)
- Oracle marks target with phase flip
- Diffusion amplifies marked amplitude
- Optimal iterations \\(\\approx \\pi/4\\sqrt{N/M}\\)
- Works by amplitude amplification, not parallelism

Grover's algorithm is your first taste of quantum speedup. It shows how quantum mechanics can outperform classical computation.

______________________________________________________________________

## Try These On QCoder

- [QPC003 B6: Grover's Algorithm I](https://www.qcoder.jp/en/contests/QPC003/problems/B6)
- [QPC003 B8: Grover's Algorithm II](https://www.qcoder.jp/en/contests/QPC003/problems/B8)
- [QPC003 Ex2: Find Hidden Number](https://www.qcoder.jp/en/contests/QPC003/problems/Ex2)

______________________________________________________________________

*Next: [Quantum Fourier Transform](./qft.md)*
