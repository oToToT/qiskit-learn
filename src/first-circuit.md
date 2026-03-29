# Your First Circuit

The first Qiskit object you need is `QuantumCircuit`.

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.h(0)
print(qc)
```

This says:

- create a one-qubit circuit
- apply a Hadamard gate to qubit `0`

Starting from `|0>`, the Hadamard creates the state

\\[ \frac{|0\rangle + |1\rangle}{\sqrt{2}} \\]

That state is often called the plus state.

## A circuit is a recipe, not a state

This distinction is worth learning immediately.

- the circuit is the recipe
- the statevector is the state produced by the recipe
- measurement turns the state into classical data

Beginners often blend these together and then get confused about what a gate is changing.

## Look at the state before measuring

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.h(0)

state = Statevector.from_instruction(qc)
print(state)
print(state.draw("text"))
```

This is the most important beginner debugging move in Qiskit. Before asking what you will observe, ask what state you built.

## Two tiny examples that already matter

Preparing `|1>`:

```python
qc = QuantumCircuit(1)
qc.x(0)
print(Statevector.from_instruction(qc))
```

Preparing the plus state:

```python
qc = QuantumCircuit(1)
qc.h(0)
print(Statevector.from_instruction(qc))
```

The point is not that these circuits are hard. The point is that you are starting to connect gate actions to states.

## Learn to predict before you run

Try this:

```python
qc = QuantumCircuit(1)
qc.x(0)
qc.h(0)
print(Statevector.from_instruction(qc))
```

Before running it, ask:

- what state does `x` create?
- what does `h` do to that state?

That habit will scale all the way to Grover, QFT, and QML circuits.

## Checkpoint Exercises

1. Prepare `|1>`.
2. Prepare the plus state.
3. Apply `x` then `h` and inspect the resulting state.
4. Build two different circuits that end in the same state.

## Try These On QCoder

- [QPC001 A1, Generate State |1>](https://www.qcoder.jp/en/contests/QPC001/problems/A1)
- [QPC001 A2, Generate Plus state](https://www.qcoder.jp/en/contests/QPC001/problems/A2)
- [QPC002 B1, Generate State e^(i theta)|0>](https://www.qcoder.jp/en/contests/QPC002/problems/B1)
