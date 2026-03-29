# Reflections And Amplitude Amplification

Once you understand phase flips, the next step is reflections.

## Reflection about a state

A reflection operator is usually written as

$$
I - 2|\psi\rangle\langle\psi|
$$

Geometrically, it flips the sign of the component along a chosen direction.

## Why reflections matter

Grover's algorithm is built from two reflections:

- one that marks good states
- one that reflects about the average amplitude

If you understand reflections, Grover stops being a memorized recipe.

## Practical construction trick

If you know how to prepare $|\psi\rangle$ from $|0...0\rangle$, then you can often build a reflection about $|\psi\rangle$ by:

1. preparing $|\psi\rangle$
2. reflecting about $|0...0\rangle$
3. unpreparing

That is a very general Qiskit pattern.

## Checkpoint Exercises

1. Build a reflection about `|11>`.
2. Build a reflection about the plus state on one qubit.
3. Build a reflection about a Bell state using prepare-reflect-unprepare.
4. Explain why reflections are natural ingredients for search algorithms.

## QCoder Connections

- QPC003 B4, "Reflection Operator I"
- QPC003 B5, "Reflection Operator II"
- QPC003 B7, "Reflection Operator III"
