# A Productive Qiskit Workflow

Writing circuits is only half the job. The other half is debugging and organizing them.

## A good local workflow

For small and medium circuits:

1. write a circuit constructor function
2. inspect the exact state with `Statevector`
3. sample with `StatevectorSampler`
4. compare against the intended mathematical action

## Reusable building blocks

As your circuits grow, use:

- helper functions
- named subcircuits
- `compose`
- controlled versions of existing subcircuits

This matters for QFT blocks, oracles, reflections, and variational ansatzes.

## Parameterized circuits

Many useful circuits should not hard-code every angle.

Use parameters when:

- the same circuit shape is reused with many angles
- you are doing variational optimization
- a QCoder problem gives symbolic or input-dependent rotations

## What to inspect when something is wrong

Check these in order:

1. qubit ordering
2. missing inverse or uncompute
3. wrong control or target
4. phase sign mistakes
5. measurement added too early

That checklist catches a lot of beginner bugs.

## Checkpoint Exercises

1. Refactor one previous chapter's solution into a reusable helper.
2. Write a small test harness that checks all 2-qubit basis inputs.
3. Parameterize a one-qubit rotation circuit and evaluate it at two angles.
4. Build a prepare-reflect-unprepare helper and reuse it for two targets.
