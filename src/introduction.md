# Qiskit for Curious Programmers

This book teaches Qiskit the way many programmers actually learn difficult technical subjects: one mental model at a time, with short circuits, repeated patterns, and deliberate practice.

The official Qiskit documentation is excellent reference material. It is not optimized for a first pass through the subject. QCoder, on the other hand, is unusually good at forcing beginners to turn vague understanding into circuits that actually work. This book tries to combine the strengths of both:

- the precision of Qiskit's APIs
- the progression and pressure of QCoder problems
- the pacing of a beginner-friendly programming book

The target reader is comfortable with Python and new to quantum programming. A physics background helps, but it is not required.

## What you should gain from this book

By the end, you should be able to:

- read and write nontrivial circuits in Qiskit
- reason about amplitudes, phases, and measurement outcomes
- debug circuits with `Statevector` and sampling primitives
- build state-preparation routines, oracles, reflections, QFT blocks, and simple arithmetic circuits
- recognize the circuit patterns behind major QCoder problem families
- write starter quantum machine learning code without getting lost in the circuit layer

That is a realistic goal. It is also a strong one.

## What this book does not promise

No single book makes someone instantly strong at every quantum algorithm.

What this book can do is bring you to the point where:

- official Qiskit tutorials stop feeling hostile
- QCoder problems look structured instead of mysterious
- you can design and test your own circuits instead of only copying them

## The teaching philosophy

This book is built on a few strong opinions.

## Circuits before hardware

You should first understand what a circuit does mathematically. Hardware noise, transpilation, and backend management matter, but they are distractions if you still confuse state preparation with measurement.

## Exact simulation before repeated sampling

Before asking what outcomes appear, ask what state you created.

That is why the book keeps coming back to:

- `Statevector.from_instruction`
- basis labels
- relative phase
- reversible mappings

## Patterns before prestige algorithms

It is better to deeply understand:

- basis preparation
- superposition
- entanglement
- oracles
- reflections

than to vaguely memorize Grover, QFT, or QML buzzwords.

Those bigger topics become approachable only after the small patterns are solid.

## Before you begin

This book works best when you treat it like a lab manual rather than a novel.

The concrete workflow for reading chapters, running code, and using the exercises is in [How To Use This Book](./how-to-use-this-book.md).
