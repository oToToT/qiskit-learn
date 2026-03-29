# Grover Search

Grover is a perfect beginner algorithm because it is both important and readable.

It is also a good test of whether you actually understand the earlier chapters. Grover combines:

- uniform superposition
- phase marking
- reflections
- iteration count

If any one of those ideas is weak, Grover becomes a ritual instead of an algorithm.

## The two-step loop

For a small search space:

1. mark the good state with an oracle
2. apply the diffusion operator

That combination amplifies the marked state's amplitude.

## A minimal 2-qubit example

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

## What the diffusion step is really doing

The diffusion block is often described as "reflection about the average."

That is not poetry. It is the right mental model.

Starting from the uniform superposition, the oracle makes the marked branch negative. The diffusion step then reflects amplitudes around their mean value, which increases the marked amplitude and decreases the others.

## Inspecting Grover without measurement

You should also look at Grover with exact simulation:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.h([0, 1])
qc.cz(0, 1)
qc.h([0, 1])
qc.x([0, 1])
qc.cz(0, 1)
qc.x([0, 1])
qc.h([0, 1])

print(Statevector.from_instruction(qc))
```

This lets you see the amplitude amplification directly instead of only as a final histogram.

## One iteration is not a universal rule

For tiny examples, one iteration can be enough. In larger spaces, the best number of iterations depends on the fraction of marked states.

The high-level idea is:

- too few iterations and the marked amplitude is still small
- too many iterations and you rotate past the target

Even if you do not study the full derivation yet, you should know that the iteration count is part of the algorithm design.

## A good debugging decomposition

When Grover fails, test each component separately:

1. does the oracle flip only the intended branch?
2. does the diffusion step preserve the start state subspace correctly?
3. after one iteration, do the amplitudes move in the direction you expect?

That decomposition is much more useful than staring at the final counts.

## Checkpoint Exercises

1. Change the oracle so the marked state is `|00>`.
2. Do the same for `|10>`.
3. Build a 3-qubit oracle for one marked state and run one Grover iteration.
4. Compare the statevector before and after the diffusion step.

## Try These On QCoder

- [QPC003 B6, Grover's Algorithm I](https://www.qcoder.jp/en/contests/QPC003/problems/B6)
- [QPC003 B8, Grover's Algorithm II](https://www.qcoder.jp/en/contests/QPC003/problems/B8)
- [QPC003 Ex2, Find Hidden Number](https://www.qcoder.jp/en/contests/QPC003/problems/Ex2)
