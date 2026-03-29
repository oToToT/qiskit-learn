# A Productive Qiskit Workflow

Writing circuits is only half the job. The other half is debugging, testing, and structuring them so you can keep going.

## A good local workflow

For small and medium circuits:

1. write a circuit constructor function
2. inspect the exact state with `Statevector`
3. sample with `StatevectorSampler`
4. compare against the intended mathematical action

That is the core loop of this book because it works.

## Write functions, not only loose notebook cells

Even for tiny experiments, it helps to write:

```python
from qiskit import QuantumCircuit

def bell_state() -> QuantumCircuit:
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    return qc
```

This gives you something you can test, compose, invert, and control.

## Use exact simulation aggressively

```python
from qiskit.quantum_info import Statevector

state = Statevector.from_instruction(bell_state())
print(state)
```

Do this early and often. If the exact state is wrong, repeated sampling will only hide the reason.

## Sample only after you know what you expect

```python
from qiskit.primitives import StatevectorSampler

qc = bell_state()
qc.measure_all()

sampler = StatevectorSampler()
result = sampler.run([qc], shots=256).result()
print(result[0].data.meas.get_counts())
```

Sampling is a confirmation step, not your primary reasoning tool.

## Reusable building blocks

As your circuits grow, use:

- helper functions
- named subcircuits
- `compose`
- `inverse`
- controlled versions of existing subcircuits

This matters for QFT blocks, oracles, reflections, and variational ansatzes.

## Parameters keep circuits honest

Many useful circuits should not hard-code every angle.

Use parameters when:

- the same circuit shape is reused with many angles
- you are doing variational optimization
- a QCoder problem gives symbolic or input-dependent rotations

```python
from qiskit import QuantumCircuit
from qiskit.circuit import Parameter

theta = Parameter("theta")
qc = QuantumCircuit(1)
qc.ry(theta, 0)

print(qc)
print(qc.assign_parameters({theta: 1.234}))
```

This is the beginning of a good workflow for QML and more advanced algorithm design.

## Testing small reversible circuits

For arithmetic and oracle circuits, a tiny test harness is often enough:

1. prepare each small basis input
2. apply the circuit
3. inspect the resulting basis state or phase
4. compare with the intended mapping

Do not wait until a contest submission to discover that your qubit order was wrong.

## What to inspect when something is wrong

Check these in order:

1. qubit ordering
2. missing inverse or uncompute
3. wrong control or target
4. phase sign mistakes
5. measurement added too early

That checklist catches a large fraction of beginner bugs.

## Checkpoint Exercises

1. Refactor one previous chapter's solution into a reusable helper.
2. Write a small test harness that checks all 2-qubit basis inputs.
3. Parameterize a one-qubit rotation circuit and evaluate it at two angles.
4. Build a prepare-reflect-unprepare helper and reuse it for two targets.
