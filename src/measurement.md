# Measurement: From Quantum to Classical

In the last chapter, we created superposition and inspected the statevector. Now let's understand what happens when we actually *measure* a qubit.

## The Measurement Problem

Quantum states are not directly observable. When you measure a qubit:

1. You get a classical result: `0` or `1`
1. The probability of each outcome comes from the amplitudes
1. The state collapses to the measured value

```
Before: α|0⟩ + β|1⟩     ──MEASURE──>    |0⟩  (prob |α|²)
                                         |1⟩  (prob |β|²)
```

## Two Different Questions

These are fundamentally different questions:

| Question | Tool | Answer |
|----------|------|--------|
| "What state is the qubit in?" | `Statevector` | Exact amplitudes and phases |
| "What do I observe when I measure?" | Sampling | Probabilistic counts |

You need both perspectives to understand quantum circuits.

## Sampling with StatevectorSampler

Qiskit provides `StatevectorSampler` for simulating measurements:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

# Create a circuit: Hadamard on one qubit
qc = QuantumCircuit(1)
qc.h(0)

# Add measurements
qc.measure_all()

# Sample with 1000 shots
sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()

# Get the counts
counts = result[0].data.meas.get_counts()
print(counts)
```

Typical output (will vary due to randomness):

```
{'0': 497, '1': 503}
```

Close to 50-50, as expected for a Hadamard on \\(|0\\rangle\\).

## Running Multiple Times

Each run of the same circuit gives slightly different counts due to randomness:

```python
# Run the same circuit 4 times with 100 shots each
for i in range(4):
    result = sampler.run([qc], shots=100).result()
    counts = result[0].data.meas.get_counts()
    print(f"Run {i+1}: {counts}")
```

You might see:

```
Run 1: {'0': 52, '1': 48}
Run 2: {'1': 55, '0': 45}
Run 3: {'0': 51, '1': 49}
Run 4: {'1': 53, '0': 47}
```

The counts fluctuate around 50-50. The *average* of many runs approaches the true probability.

## Why Not Just Measure?

If measurement gives random results, why do we care about quantum computing?

Because:

1. **We control the probabilities** through circuit design
1. **Interference** can make "wrong" answers cancel out
1. **Entanglement** creates correlations that classical systems can't match

Measurement is the *end* of computation, not the beginning.

## The Statevector Tells the Truth

If you only look at measurement counts, you miss important information:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create two circuits with different phases
qc1 = QuantumCircuit(1)
qc1.h(0)        # |+⟩ = (|0⟩ + |1⟩) / √2

qc2 = QuantumCircuit(1)
qc2.h(0)        # |+⟩
qc2.z(0)        # Apply Z: (|0⟩ - |1⟩) / √2 = |−⟩

# Look at the states
print("State 1 (|+⟩):", Statevector(qc1))
print("State 2 (|−⟩):", Statevector(qc2))
```

Output:

```
State 1 (|+⟩): Statevector([ 0.70710678+0.j,  0.70710678+0.j], dims=(2,))
State 2 (|−⟩): Statevector([ 0.70710678+0.j, -0.70710678+0.j], dims=(2,))
```

The states are different (different phase), but if you measure either one, you'll get 50-50 counts.

**The phase difference is invisible to measurement but crucial for interference.**

## A Two-Qubit Example

Let's see measurement on entangled states:

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

# Create a Bell state: (|00⟩ + |11⟩) / √2
qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

# Sample many times
sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()
counts = result[0].data.meas.get_counts()
print(counts)
```

Typical output:

```
{'11': 502, '00': 498}
```

You *only* see `00` and `11`. Never `01` or `10`.

This is **quantum correlation**—the measurement outcomes are linked even though neither outcome is predetermined.

## When Measurement Destroys Information

If you measure in the middle of a circuit, you destroy the quantum state:

```python
# What NOT to do: measure too early
qc = QuantumCircuit(2)
qc.h(0)    # Creates superposition
qc.cx(0, 1)  # Creates entanglement
qc.measure_all()  # ← Measuring here destroys everything!

# vs. What you want: measure at the end
qc_good = QuantumCircuit(2)
qc_good.h(0)
qc_good.cx(0, 1)
# No measurement yet—inspect the state first!
state = Statevector(qc_good)
print("Before measurement:", state)
```

The good circuit shows the full entangled state. The measured circuit shows only collapsed results.

## The Workflow: Statevector First

Here's the recommended workflow for any circuit:

1. Write the circuit
1. Inspect Statevector()
   - Are the amplitudes correct?
   - Are the phases correct?
1. Predict what measurement will show
1. Add measurements and sample
1. Verify counts match your prediction

This order prevents the "I have no idea why this is broken" debugging spiral.

## Checkpoint Exercises

### Exercise 1

Create a circuit that always measures `1`. Verify with 100 shots.

### Exercise 2

Create a circuit with a 25-75 split between `0` and `1`.

### Exercise 3

Build two circuits that produce identical measurement counts but different statevectors.

### Exercise 4

Create a Bell state and list all possible measurement outcomes.

### Exercise 5

Explain why measuring a qubit in superposition "destroys" information.

## The Deep Truth

Measurement isn't just reading a value. In quantum mechanics, measurement:

1. Forces the system to choose
1. Destroys superposition
1. Creates randomness where there was none

This is why quantum computing is powerful and subtle: you must carefully design circuits so that the "right" answers become more probable, even though any individual measurement is random.

## Summary

In this chapter, you learned:

- Measurement collapses quantum states to classical outcomes
- `StatevectorSampler` simulates measurement statistics
- Counts are probabilistic but average to true probabilities
- Phase differences are invisible to immediate measurement
- The statevector-first workflow prevents many bugs

Measurement is how quantum computation becomes classical information. But understanding what happens *before* measurement is the key to designing useful quantum circuits.

> **Official Documentation:** For more on measurement in Qiskit, see the [Qiskit documentation on measurement](https://docs.quantum.ibm.com/build/noise-models). The [StatevectorSampler](https://docs.quantum.ibm.com/api/qiskit/qiskit.primitives.StatevectorSampler) is the recommended way to simulate measurement statistics.

______________________________________________________________________

## Try These On QCoder

- [QPC001 A3: Generate Minus state](https://www.qcoder.jp/en/contests/QPC001/problems/A3)
- [QPC003 B2: Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC005 B1: Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)

______________________________________________________________________

*Next: [Qubit Ordering: The Endian Problem](./qubit-ordering.md)*
