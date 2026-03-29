# State Preparation Patterns

QCoder loves state-preparation tasks because they expose whether you understand gates as transformations rather than names.

This chapter is about designing states on purpose.

## Pattern 1: basis states

To prepare `|101>`, flip the right qubits:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)
qc.x(0)
qc.x(2)
print(Statevector.from_instruction(qc))
```

This is the easiest pattern, but it still forces you to respect qubit ordering.

## Pattern 2: equal superposition

To create an equal superposition over all `n`-qubit basis states:

```python
from qiskit import QuantumCircuit

n = 3
qc = QuantumCircuit(n)
for i in range(n):
    qc.h(i)
```

This is the most common starting state in search-style algorithms.

## Pattern 3: amplitudes first, routing second

Many hand-built two- and three-qubit states are easiest to construct by:

1. setting amplitudes on one qubit
2. using controlled gates to route amplitude into the desired basis states
3. fixing signs after the magnitudes are correct

That order matters because sign mistakes are easier to fix after the amplitude layout is already right.

## Pattern 4: signs after probabilities

Two states can share the same measurement distribution and still differ meaningfully:

\\[ \frac{|00\rangle + |11\rangle}{\sqrt{2}} \quad \text{and} \quad \frac{|00\rangle - |11\rangle}{\sqrt{2}} \\]

That is when `z`, `cz`, and basis changes matter.

## Pattern 5: build symmetry deliberately

States like

\\[ \frac{|100\rangle + |010\rangle + |001\rangle}{\sqrt{3}} \\]

are good training because they force you to think in amplitudes, not just bit flips.

They also start teaching you a useful habit: name the target state in math before you write any gates.

## A concrete two-qubit construction

Suppose you want:

\\[ \sqrt{\frac{3}{4}}|00\rangle + \frac{1}{2}|11\rangle \\]

One clean strategy is:

1. use `ry` on qubit `0` to split amplitude between a `0` branch and a `1` branch
2. use `cx(0, 1)` to copy the branch label into qubit `1`

```python
from math import pi
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.ry(pi / 3, 0)
qc.cx(0, 1)
print(Statevector.from_instruction(qc))
```

This does not solve every state-preparation problem, but it shows a pattern that comes up constantly: prepare one branch variable, then route it with controls.

## Manual design versus `prepare_state`

Qiskit has convenience methods that can load arbitrary states. Those are useful tools, but they are bad teachers.

Early in your learning, prefer manual constructions because they teach:

- qubit order
- amplitude planning
- basis changes
- sign correction

Later, when you already understand the circuit, a convenience routine can be fine.

## Checkpoint Exercises

1. Prepare the Bell state.
2. Prepare the sign-flipped Bell state.
3. Prepare the 3-qubit one-hot superposition above.
4. Prepare a uniform superposition on four qubits.

## Try These On QCoder

- [QPC001 A5, Generate State (sqrt(3)/2)|100> + (1/2)|011>](https://www.qcoder.jp/en/contests/QPC001/problems/A5)
- [QPC003 A3, Generate State (|100> + |010> + |001>)/sqrt(3)](https://www.qcoder.jp/en/contests/QPC003/problems/A3)
- [QPC003 A4, A5, and A6](https://www.qcoder.jp/en/contests/QPC003)
