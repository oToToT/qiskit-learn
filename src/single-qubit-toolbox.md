# The Single-Qubit Toolbox

Most beginner QCoder-style tasks start by asking you to create a small target state. That means you need a compact gate vocabulary.

## The gates to learn first

- `x`: flips `|0>` and `|1>`
- `z`: changes the sign of `|1>`
- `h`: mixes basis states
- `rx`, `ry`, `rz`: smooth rotations

If you understand those well, a lot of beginner circuits stop looking magical.

## A useful pattern

`ry(theta)` is often the easiest way to prepare a one-qubit state with real amplitudes.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from math import pi

qc = QuantumCircuit(1)
qc.ry(pi / 3, 0)

state = Statevector.from_instruction(qc)
print(state)
```

That gives you a state of the form:

\[
\cos(\theta/2)|0\rangle + \sin(\theta/2)|1\rangle
\]

## Why `z` feels weird at first

`z` does nothing visible to `|0>`.

```python
qc = QuantumCircuit(1)
qc.z(0)
```

Measured directly, this still gives `0` every time.

But if the qubit is already in superposition, `z` changes relative phase, and relative phase changes future interference.

That is why `h -> z -> h` is not useless. It acts like `x`.

## DIY

Try to build these states on one qubit:

1. \((|0\rangle - |1\rangle)/\sqrt{2}\)
2. \(-|1\rangle\)
3. a state where `P(1) = 3/4`

For the third one, prefer a rotation gate instead of guessing with many gates.
