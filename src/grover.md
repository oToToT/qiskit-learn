# A Small Grover Search

Grover's algorithm is a great first "real" algorithm because it is still readable as a circuit.

## The idea

Start with a uniform superposition over all candidates.

Then repeat:

1. mark the good answer with an oracle
2. apply the diffusion step to amplify it

For two qubits and one marked state, one Grover iteration is enough.

## A minimal example

This version marks `|11>`.

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
job = sampler.run([qc], shots=256)
result = job.result()
print(result[0].data.meas.get_counts())
```

You should see `"11"` dominate.

## Why this chapter matters

Grover combines most of the early book:

- superposition
- phase marking
- controlled gates
- interference

If this chapter makes sense, the earlier pieces are starting to connect.

## DIY

Modify the oracle so Grover searches for:

1. `|00>`
2. `|10>`
3. a marked state of your choice on three qubits

For the three-qubit version, do not worry about optimal iteration count yet. Focus on writing the oracle and the diffusion pattern clearly.
