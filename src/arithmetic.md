# Arithmetic And Modular Thinking

A surprising number of contest problems stop being scary once you reframe them as reversible arithmetic.

## Why arithmetic matters

Arithmetic circuits show up in:

- modular algorithms
- phase estimation variants
- hidden-number style tasks
- more advanced QCoder contest problems

## The beginner shift

At first, you learn gates.

Then you learn state preparation and oracles.

Then you learn that many harder circuits are just structured data movement:

- add a constant
- shift a register
- swap positions
- apply a permutation reversibly

## Design advice

When building arithmetic circuits:

1. define the register layout explicitly
2. state the intended mapping on basis states
3. test small inputs exhaustively
4. uncompute any temporary work registers

This is much more reliable than guessing from circuit diagrams.

## Checkpoint Exercises

1. Implement a 2-qubit swap and verify it on all basis states.
2. Implement a cyclic shift on a small register.
3. Define a reversible map for adding a known constant to a tiny register.
4. Write tests that compare the output basis state for every input.

## QCoder Connections

- QPC002 B3, "SWAP Qubits"
- QPC002 B5 through B8, the arithmetic sequence
- QPC005 B2 and later modular/Fourier-basis problems
