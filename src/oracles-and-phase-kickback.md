# Oracles And Phase Kickback

This is usually where beginners start feeling that quantum circuits became "real algorithms" instead of gate puzzles.

## What an oracle does

In beginner quantum algorithms, an oracle is often a circuit that marks good answers.

A common marking rule is:

\[
|x\rangle \mapsto (-1)^{f(x)} |x\rangle
\]

If `f(x) = 1`, that basis state gets a minus sign.

No measurement happens here. We only change phase.

## Why that matters

Changing a sign seems harmless if you only measure immediately afterward.

But after more interference, marked states can become easier to detect.

That is the engine behind Grover-style thinking.

## Tiny example

Suppose you want to mark `|11>` on two qubits.

```python
from qiskit import QuantumCircuit

oracle = QuantumCircuit(2)
oracle.cz(0, 1)
```

This flips the phase of `|11>` and leaves the other basis states alone.

## Phase kickback in one sentence

A control qubit can pick up phase information from a target prepared in a suitable eigenstate.

For a beginner, the practical takeaway is enough:

- phase can be written without measurement
- controlled operations are how many algorithms move information around

## DIY

Build small marking circuits that phase-flip exactly:

1. `|11>` on two qubits
2. `|10>` on two qubits
3. one chosen basis state on three qubits

Hint: for states other than all-ones, use `x` gates before and after the multi-controlled phase logic.
