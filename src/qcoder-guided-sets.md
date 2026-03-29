# Guided Exercise Sets

These are the main practice sets for the book.

## Set A: chapter checkpoints

After each chapter, solve its checkpoint exercises without opening the chapter again.

Goal:

- move from recognition to recall

## Set B: QCoder ladder

Solve the following groups in order.

### Group 1

- [QPC001 A1 to A5](https://www.qcoder.jp/en/contests/QPC001)
- [QPC003 A1 to A3](https://www.qcoder.jp/en/contests/QPC003)

Checkpoint:

- you can prepare target states without trial-and-error gate spam
- you can explain the role of every gate in each solution

### Group 2

- [QPC002 B2](https://www.qcoder.jp/en/contests/QPC002/problems/B2)
- [QPC003 B2](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)

Checkpoint:

- you can move comfortably between bit-flip and phase-flip formulations
- you can explain phase kickback without hand-waving

### Group 3

- [QPC003 B4 to B8](https://www.qcoder.jp/en/contests/QPC003)

Checkpoint:

- you can explain every Grover component separately
- you can diagnose which step breaks when the output is wrong

### Group 4

- [QPC002 B3 to B8](https://www.qcoder.jp/en/contests/QPC002)
- [QPC005 B1 to B2](https://www.qcoder.jp/en/contests/QPC005)

Checkpoint:

- you can reason about Fourier-basis actions and reversible register manipulation
- you can write small exhaustive tests for arithmetic circuits

## Set C: algorithm builder drills

Write your own small problems:

1. mark one basis state on three qubits
2. build the corresponding Grover step
3. replace the oracle with a different marked set
4. swap in a parameterized state-preparation block

That is how you stop solving only named contest problems and start designing circuits.

## Submission discipline

Before you submit a QCoder solution, ask:

1. did I verify basis ordering?
2. did I separate global phase from relative phase?
3. did I test all small basis inputs if the circuit is arithmetic or reversible?
4. can I explain why the solution works without reading the code line by line?
