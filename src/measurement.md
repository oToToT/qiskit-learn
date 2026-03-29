# Measurement And Sampling

Quantum states are not classical values. Measurement turns amplitudes into sampled outcomes.

## Exact amplitudes versus observed counts

These questions are different:

- what state is the circuit in right now?
- what bitstrings appear if I measure repeatedly?

Qiskit gives you tools for both, and you should get used to using both.

## Sampling with `StatevectorSampler`

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

Counts are empirical data. They fluctuate with the number of shots. The statevector does not.

## Why beginners should not measure too early

If you measure too soon, you destroy the information you were trying to understand.

A better workflow is:

1. inspect the exact state
2. predict the measurement distribution
3. sample counts to confirm the prediction

That order is especially important once relative phase enters the story.

## A first phase surprise

These circuits produce the same one-shot measurement distribution if you measure immediately:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc1 = QuantumCircuit(1)
qc1.h(0)

qc2 = QuantumCircuit(1)
qc2.h(0)
qc2.z(0)

print(Statevector.from_instruction(qc1))
print(Statevector.from_instruction(qc2))
```

Their states are:

\\[ \frac{|0\rangle + |1\rangle}{\sqrt{2}} \quad \text{versus} \quad \frac{|0\rangle - |1\rangle}{\sqrt{2}} \\]

If you measure right away, both give 50-50 counts. But they are not the same state, and later gates can expose that difference.

## A two-qubit example

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=1000).result()
print(result[0].data.meas.get_counts())
```

You should only see `"00"` and `"11"`.

This is your first example of a state whose measurement outcomes are correlated.

## What counts can and cannot tell you

Counts are good for:

- checking whether impossible outcomes really are impossible
- estimating probabilities
- validating the final behavior of a circuit

Counts are bad for:

- identifying relative phase directly
- explaining why a circuit works
- debugging intermediate structure

That is why statevector inspection stays so central in this book.

## Checkpoint Exercises

1. Prepare a circuit that always measures `1`.
2. Prepare a circuit with a 50-50 split between `0` and `1`.
3. Build two different circuits with the same counts but different states.
4. Measure a Bell state and list which two outcomes appear.

## Try These On QCoder

- [QPC001 A3, Generate Minus state](https://www.qcoder.jp/en/contests/QPC001/problems/A3)
- [QPC003 B2, Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC005 B1, Action in the Fourier Basis](https://www.qcoder.jp/en/contests/QPC005/problems/B1)
