# Reflections And Amplitude Amplification

Once you understand phase flips, the next step is reflections.

## Reflection about a state

A reflection operator is usually written as

\\[ I - 2|\psi\rangle\langle\psi| \\]

Geometrically, it flips the sign of the component along a chosen direction and leaves the orthogonal subspace alone.

You do not need full linear-algebra fluency to use this idea productively. You do need to recognize when a circuit is implementing a reflection.

## Why reflections matter

Grover's algorithm is built from two reflections:

- one that marks good states
- one that reflects about the prepared starting state

If you understand reflections, Grover stops being a memorized recipe.

## Reflection about `|11>`

On two qubits, a reflection about `|11>` is just a selective sign flip on that basis state:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Operator

qc = QuantumCircuit(2)
qc.cz(0, 1)
print(Operator(qc).data)
```

This leaves `|00>`, `|01>`, and `|10>` alone and flips the sign of `|11>`.

## Reflection about `|0...0>`

A common building block is a reflection about the all-zero state. In small examples, you can think of it as:

- flip the sign of `|0...0>`
- leave the orthogonal basis states alone

By surrounding that operation with state preparation and its inverse, you can reflect about a much more interesting state.

## The prepare-reflect-unprepare pattern

If you know how to prepare `|\psi\rangle` from `|0...0\rangle`, then you can build a reflection about `|\psi\rangle` by:

1. preparing `|\psi\rangle`
2. reflecting about `|0...0\rangle`
3. unpreparing

In math:

\\[ U\left(I - 2|0...0\rangle\langle 0...0|\right)U^\dagger = I - 2|\psi\rangle\langle\psi| \\]

where `U|0...0> = |\psi>`.

## A one-qubit example

To reflect about `|+>`:

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)
qc.h(0)
```

Because `h` prepares `|+>` from `|0>`, the sequence `h -> z -> h` gives a reflection about `|+>`, which is also the operator `x`.

## Why this matters for debugging

When a Grover-like circuit fails, one of the most useful questions is:

- is my oracle really a reflection?
- is my diffusion step really a reflection about the start state?

That framing is much more robust than staring at a gate list.

## Checkpoint Exercises

1. Build a reflection about `|11>`.
2. Build a reflection about the plus state on one qubit.
3. Build a reflection about a Bell state using prepare-reflect-unprepare.
4. Explain why reflections are natural ingredients for search algorithms.

## Try These On QCoder

- [QPC003 B4, Reflection Operator I](https://www.qcoder.jp/en/contests/QPC003/problems/B4)
- [QPC003 B5, Reflection Operator II](https://www.qcoder.jp/en/contests/QPC003/problems/B5)
- [QPC003 B7, Reflection Operator III](https://www.qcoder.jp/en/contests/QPC003/problems/B7)
