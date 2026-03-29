# Quantum Fourier Transform

QFT is the next big milestone after Grover because it teaches you to think in phase structure rather than only marked states.

## What QFT does

At a high level, the Quantum Fourier Transform changes basis so that periodic or phase-encoded structure becomes easier to read.

You do not need the full abstract formalism to begin using it.

## The small-circuit view

A standard QFT circuit is built from:

- Hadamards
- controlled phase rotations
- optional swaps to reverse qubit order

That already tells you something important: QFT is a basis-change machine built out of simple local pieces.

## Why QFT and qubit order are linked

Many QFT confusions are not about Fourier analysis. They are about:

- little-endian ordering
- where the swaps belong
- whether the inverse QFT is being used

This is one reason the earlier basis-order chapter matters so much.

## Minimal study goals

Before moving on, you should be able to:

- explain why controlled phase gates appear in QFT
- distinguish QFT from inverse QFT
- say when trailing swaps matter

## Checkpoint Exercises

1. Write a 2-qubit QFT circuit by hand.
2. Compare your circuit to `QFT` from Qiskit if you decide to inspect the library implementation later.
3. Remove the final swaps and describe what changes.
4. Apply QFT and inverse QFT in sequence and verify you recover the input state.

## QCoder Connections

- QPC002 B4, "Quantum Fourier Transform"
- QPC005 B1, "Action in the Fourier Basis"
- QPC005 B2, "Cyclic Shift in the Fourier Basis"
