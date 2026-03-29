# Reversible Logic And Oracles

Quantum circuits cannot freely erase information. That is why reversible logic shows up everywhere.

## A useful mindset

When a classical step says "compute a value," a quantum version often has to say:

- compute it reversibly
- use it
- uncompute temporary garbage

This matters for arithmetic, oracles, and many QCoder implementation tasks.

## Bit-flip oracles

A bit-flip oracle changes an ancilla when a condition is true:

\\[ |x\rangle|b\rangle \mapsto |x\rangle|b \oplus f(x)\rangle \\]

These are often easier to think about first because they look more classical.

For example, to flip an ancilla when both data qubits are `1`:

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(3)
qc.ccx(0, 1, 2)
```

If qubits `0` and `1` store the input and qubit `2` is the ancilla, this computes logical AND into the ancilla.

## Phase oracles

A phase oracle marks good states by a sign:

\\[ |x\rangle \mapsto (-1)^{f(x)}|x\rangle \\]

Phase oracles are central in Grover-style algorithms because they mark states without needing a classical output wire in the final description.

For `|11>`, the simplest phase oracle is just:

```python
qc = QuantumCircuit(2)
qc.cz(0, 1)
```

## Turning one kind into the other

If you prepare an ancilla in the minus state

\\[ \frac{|0\rangle - |1\rangle}{\sqrt{2}} \\]

then a bit-flip on that ancilla acts like a phase flip on the control condition. This is the practical core of phase kickback.

## A minimal bit-flip to phase-flip conversion

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(3)

# Prepare a superposition on the two data qubits.
qc.h([0, 1])

# Prepare the ancilla in |->
qc.x(2)
qc.h(2)

# Flip ancilla iff the input is 11.
qc.ccx(0, 1, 2)

print(Statevector.from_instruction(qc))
```

If you inspect the resulting amplitudes, the `|11>` branch on the data register picks up a minus sign.

## Uncomputation is not optional

Suppose you compute a helper value into a work qubit and then keep going. If you never uncompute, that work qubit can stay entangled with your data and quietly break the intended algorithm.

The safe pattern is:

1. compute
2. use
3. uncompute

That pattern will appear again in arithmetic, reflections, and Grover-style blocks.

## Checkpoint Exercises

1. Write a phase oracle that marks `|11>`.
2. Write a phase oracle that marks `|10>`.
3. Write a bit-flip oracle for the same condition and compare the two.
4. Add temporary work qubits to a toy reversible computation and then uncompute them.

## Try These On QCoder

- [QPC002 B2, Phase Shift Oracle](https://www.qcoder.jp/en/contests/QPC002/problems/B2)
- [QPC003 B2, Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1, Convert Bit-Flip into Phase-Flip II](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)
