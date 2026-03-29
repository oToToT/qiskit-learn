# Grover Search

Grover is a perfect beginner algorithm because it is both important and readable.

## The two-step loop

For a small search space:

1. mark the good state with an oracle
2. apply the diffusion operator

That combination amplifies the marked state's amplitude.

## Minimal 2-qubit example

```python
from qiskit import QuantumCircuit
from qiskit.primitives import StatevectorSampler

qc = QuantumCircuit(2)
qc.h([0, 1])

# Oracle: mark |11>
qc.cz(0, 1)

# Diffusion
qc.h([0, 1])
qc.x([0, 1])
qc.cz(0, 1)
qc.x([0, 1])
qc.h([0, 1])

qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=256).result()
print(result[0].data.meas.get_counts())
```

For this case, `"11"` should dominate.

## What you should understand, not memorize

Understand these pieces separately:

- uniform superposition
- phase marking
- reflection about the average
- iteration count

If you only memorize gate strings, every variant feels new. If you understand the pieces, most variants are small edits.

## Checkpoint Exercises

1. Change the oracle so the marked state is `|00>`.
2. Do the same for `|10>`.
3. Build a 3-qubit oracle for one marked state and run one Grover iteration.
4. Compare the statevector before and after the diffusion step.

## QCoder Connections

- QPC003 B6, "Grover's Algorithm I"
- QPC003 B8, "Grover's Algorithm II"
- QPC003 Ex2, "Find Hidden Number"
