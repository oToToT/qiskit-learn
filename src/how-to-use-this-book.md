# How To Use This Book

This book is meant to be worked through, not merely read.

Every chapter is built around the same loop:

1. learn one mental model
2. type one or two short Qiskit examples
3. inspect the exact state before measuring
4. solve the chapter exercises
5. try one or two linked QCoder problems while the idea is still fresh

## The chapter pattern

Most chapters follow four layers.

## Core idea

This is the conceptual layer. Examples:

- a Hadamard is a basis change, not just a gate symbol
- `cx(control, target)` is a conditional action on the target
- a phase can be invisible in immediate measurement counts but still matter later

If you skip this layer, Qiskit becomes syntax memorization.

## Qiskit move

This is the API layer. You will repeatedly use:

- `QuantumCircuit` to write circuits
- `Statevector.from_instruction` to inspect exact amplitudes
- `StatevectorSampler` to test measurement distributions
- `compose`, `inverse`, and `control` to assemble larger circuits

The book deliberately reuses these tools until they feel routine.

## Worked pattern

This is the design layer. You should learn to recognize patterns such as:

- prepare a basis state
- create a uniform superposition
- turn a condition into a phase flip
- convert a bit-flip oracle into a phase oracle
- prepare, reflect, unprepare

Once you can name a pattern, many QCoder problems stop feeling unique.

## Exercises

The exercises are where the chapter becomes useful.

Each chapter ends with:

- checkpoint exercises that force you to restate the idea in your own code
- a short set of linked QCoder problems that use the same pattern in a less guided setting

## A practical reading strategy

Use this rhythm for each chapter:

1. read the chapter once without writing code
2. type every code block yourself
3. predict the state or counts before running anything
4. change one gate and explain what changed
5. solve at least one linked QCoder problem before moving on

That last step matters. If you only read, the material will feel clearer than it really is.

## What to do when you get stuck

When a circuit does not behave as expected, check these in order:

1. did I misunderstand qubit order?
2. am I looking at amplitudes or only at counts?
3. did I accidentally measure too early?
4. did I forget an inverse or uncompute step?
5. am I confusing relative phase with global phase?

That checklist will save you a surprising amount of time.
