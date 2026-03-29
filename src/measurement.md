# Measurement And Probabilities

Quantum states are not classical values. Measurement turns amplitudes into outcomes.

## Exact state vs sampled outcomes

These are different questions:

- "What are the amplitudes right now?"
- "What bitstrings do I see after measuring many times?"

Qiskit gives you tools for both.

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(1)
qc.h(0)
qc.measure_all()

sampler = StatevectorSampler()
job = sampler.run([qc], shots=1000)
result = job.result()
counts = result[0].data.meas.get_counts()
print(counts)
```

For a fair superposition, counts should be roughly split between `"0"` and `"1"`.

## What measurement destroys

If you measure a superposition, you do not get the amplitudes back. You get one sampled outcome.

That is why we use:

- `Statevector` when learning what a circuit means
- sampling when learning what a user would observe

## A useful experiment

Compare these two circuits:

```python
qc1 = QuantumCircuit(1)
qc1.h(0)

qc2 = QuantumCircuit(1)
qc2.h(0)
qc2.z(0)
```

Their measurement counts are the same.

Their states are not the same.

That is your first reminder that phase matters.

## DIY

Create a one-qubit circuit for each target distribution:

1. always measure `1`
2. measure `0` and `1` with equal probability
3. get the same counts as case 2, but with a different underlying statevector
