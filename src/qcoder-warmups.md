# QCoder-Style Warmups

This section is not an official QCoder mirror. It is a training set inspired by the flow common in beginner QCoder contests: short tasks first, then one new twist at a time.

## Warmup Set A: One qubit

1. Prepare `|1>`.
2. Prepare \((|0\rangle + |1\rangle)/\sqrt{2}\).
3. Prepare \((|0\rangle - |1\rangle)/\sqrt{2}\).
4. Prepare a one-qubit state with `P(1)=1/3`.

## Warmup Set B: Two qubits

1. Prepare `|10>`.
2. Prepare \((|00\rangle + |11\rangle)/\sqrt{2}\).
3. Prepare \((|01\rangle + |10\rangle)/\sqrt{2}\).
4. Mark `|11>` with a phase flip and verify it by inspecting the statevector.

## Warmup Set C: Thinking in transformations

1. Write a circuit that converts a phase flip on `|1>` into a bit flip.
2. Build a controlled operation that flips qubit `1` when qubit `0` is `1`.
3. Start from a uniform superposition and mark one state with a minus sign.

## Recommended workflow

For each problem:

1. write the target state or behavior
2. guess the smallest gate set that could work
3. inspect with `Statevector`
4. only then add measurement sampling

That workflow is closer to debugging than to guessing.
