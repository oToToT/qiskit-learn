# Measurement And Sampling

Quantum states are not classical values. Measurement turns amplitudes into sampled outcomes.

## Exact amplitudes versus observed counts

These questions are different:

- what state is the circuit in right now?
- what bitstrings appear if I measure repeatedly?

Qiskit gives you tools for both.

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(1)
qc.h(0)
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()
counts = result[0].data.meas.get_counts()
print(counts)
```

For the plus state, the counts should be close to half `"0"` and half `"1"`.

## Why beginners should not measure too early

If you measure too soon, you destroy the information you were trying to understand.

A better workflow is:

1. inspect the exact state
2. predict the measurement distribution
3. sample counts to confirm the prediction

## A first phase surprise

These circuits produce the same one-shot distribution:

```python
qc1 = QuantumCircuit(1)
qc1.h(0)

qc2 = QuantumCircuit(1)
qc2.h(0)
qc2.z(0)
```

Their counts look the same if you measure immediately.

Their states are different:

$$
\frac{|0\rangle + |1\rangle}{\sqrt{2}}
\quad\text{versus}\quad
\frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

That difference becomes visible after more gates. This is your first encounter with interference.

## Checkpoint Exercises

1. Prepare a circuit that always measures `1`.
2. Prepare a circuit with a 50-50 split between `0` and `1`.
3. Build two different circuits with the same counts but different states.
4. Measure a Bell state and list which two outcomes appear.

## QCoder Connections

- QPC001 A3, "Generate Minus state"
- QPC003 B2, "Convert Bit-Flip into Phase-Flip I"
- QPC005 B1, "Action in the Fourier Basis"
