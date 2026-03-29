# Reversible Logic And Oracles

Quantum circuits cannot freely erase information. That is why reversible logic shows up everywhere.

## A useful mindset

When a classical step says "compute a value," a quantum version often has to say:

- compute it reversibly
- use it
- uncompute temporary garbage

This matters for arithmetic, oracles, and many QCoder implementation tasks.

## Bit-flip oracles

A bit-flip oracle changes an ancilla when a condition is true.

For example, you might map:

$$
|x\rangle|b\rangle \mapsto |x\rangle|b \oplus f(x)\rangle
$$

These are often easier to think about first because they look more classical.

## Phase oracles

A phase oracle marks good states by a sign:

$$
|x\rangle \mapsto (-1)^{f(x)}|x\rangle
$$

Phase oracles are central in Grover-style algorithms.

## Turning one kind into the other

If you prepare an ancilla in the minus state

$$
\frac{|0\rangle - |1\rangle}{\sqrt{2}},
$$

then a bit-flip on that ancilla acts like a phase flip on the control condition. This is the practical core of phase kickback.

## Checkpoint Exercises

1. Write a phase oracle that marks `|11>`.
2. Write a phase oracle that marks `|10>`.
3. Write a bit-flip oracle for the same condition and compare the two.
4. Add temporary work qubits to a toy reversible computation and then uncompute them.

## QCoder Connections

- QPC002 B2, "Phase Shift Oracle"
- QPC003 B2, "Convert Bit-Flip into Phase-Flip I"
- QPC003 Ex1, "Convert Bit-Flip into Phase-Flip II"
