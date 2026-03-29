# Controlled Gates, Correlation, And Entanglement

The `cx` gate is where many circuits stop feeling like independent one-qubit manipulations.

## Read `cx` correctly

```python
qc.cx(control, target)
```

means:

"flip the target if the control is `1`."

That sentence is enough to understand most beginner two-qubit circuits.

## A first controlled-gate sanity check

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.x(0)
qc.cx(0, 1)
print(Statevector.from_instruction(qc))
```

Because qubit `0` starts as `1`, the target qubit `1` is flipped, and the final state is `|11>`.

## The Bell state

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
print(Statevector.from_instruction(qc))
```

This prepares:

\\[ \frac{|00\rangle + |11\rangle}{\sqrt{2}} \\]

If you sample it, you only see `00` and `11`.

## Correlation versus entanglement

At a beginner level, the practical rule is:

- a correlated measurement pattern is easy to observe
- entanglement is the quantum structure underneath some of those patterns

For now, you mainly need to become fluent with constructing Bell states, measuring them, and modifying their signs with `z` or `cz`.

## Order matters

These are not the same:

```python
qc1.h(0)
qc1.cx(0, 1)

qc2.cx(0, 1)
qc2.h(0)
```

Quantum circuits are ordered transformations, not unordered gate bags.

## Control and target are not symmetric

These are also not the same:

```python
qc1.cx(0, 1)
qc2.cx(1, 0)
```

On a basis state where only one qubit is `1`, the outcomes can be completely different. That is why naming control and target clearly matters.

## A sign-flipped Bell state

Once you have the Bell state, one `z` gate changes it to:

\\[ \frac{|00\rangle - |11\rangle}{\sqrt{2}} \\]

That single sign change is invisible if you measure immediately, but it matters for later interference and basis changes.

## Checkpoint Exercises

1. Prepare `( |01> + |10> ) / sqrt(2)`.
2. Prepare `|10>` using exactly one `x` gate.
3. Build a circuit that creates a Bell state and then maps it to `( |00> - |11> ) / sqrt(2)`.
4. Compare `cx(0, 1)` and `cx(1, 0)` on the same input state.

## Try These On QCoder

- [QPC001 A4, Generate State (|10> + |11>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC001/problems/A4)
- [QPC002 B3, SWAP Qubits](https://www.qcoder.jp/en/contests/QPC002/problems/B3)
- [QPC003 A2, Generate State (|10> + |01>)/sqrt(2)](https://www.qcoder.jp/en/contests/QPC003/problems/A2)
