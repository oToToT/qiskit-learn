# QCoder Practice Tracks

This part of the book is explicitly organized around real QCoder contest progression.

It is not an official mirror of QCoder statements. It is a study map.

## Track 1: state preparation

Start here:

- [QPC001 contest page](https://www.qcoder.jp/en/contests/QPC001)
- [QPC001 A1, Generate State |1>](https://www.qcoder.jp/en/contests/QPC001/problems/A1)
- [QPC001 A2, Generate Plus state](https://www.qcoder.jp/en/contests/QPC001/problems/A2)
- [QPC001 A3, Generate Minus state](https://www.qcoder.jp/en/contests/QPC001/problems/A3)
- [QPC001 A4](https://www.qcoder.jp/en/contests/QPC001/problems/A4) and [QPC001 A5](https://www.qcoder.jp/en/contests/QPC001/problems/A5)
- [QPC003 contest page](https://www.qcoder.jp/en/contests/QPC003)
- [QPC003 A1 through A6](https://www.qcoder.jp/en/contests/QPC003)

What to practice:

- basis-state preparation
- plus/minus states
- one-hot superpositions
- sign control after amplitude control

## Track 2: phase and oracle conversion

Then work through:

- [QPC002 contest page](https://www.qcoder.jp/en/contests/QPC002)
- [QPC002 B2, Phase Shift Oracle](https://www.qcoder.jp/en/contests/QPC002/problems/B2)
- [QPC003 B2, Convert Bit-Flip into Phase-Flip I](https://www.qcoder.jp/en/contests/QPC003/problems/B2)
- [QPC003 Ex1, Convert Bit-Flip into Phase-Flip II](https://www.qcoder.jp/en/contests/QPC003/problems/Ex1)

What to practice:

- phase oracles
- minus-state ancillas
- basis changes that turn phase into bit behavior

## Track 3: reflections and Grover

Then:

- [QPC003 B4](https://www.qcoder.jp/en/contests/QPC003/problems/B4), [B5](https://www.qcoder.jp/en/contests/QPC003/problems/B5), and [B7](https://www.qcoder.jp/en/contests/QPC003/problems/B7)
- [QPC003 B6](https://www.qcoder.jp/en/contests/QPC003/problems/B6) and [B8](https://www.qcoder.jp/en/contests/QPC003/problems/B8)
- [QPC003 Ex2](https://www.qcoder.jp/en/contests/QPC003/problems/Ex2)

What to practice:

- reflections about basis and prepared states
- oracle plus diffusion decomposition
- debugging amplitude amplification step by step

## Track 4: Fourier and arithmetic

Then:

- [QPC002 B4, Quantum Fourier Transform](https://www.qcoder.jp/en/contests/QPC002/problems/B4)
- [QPC002 B5 through B8](https://www.qcoder.jp/en/contests/QPC002)
- [QPC005 contest page](https://www.qcoder.jp/en/contests/QPC005)
- [QPC005 B1 and B2](https://www.qcoder.jp/en/contests/QPC005)

What to practice:

- basis ordering
- controlled phases
- register mappings
- reversible arithmetic thinking

## Track 5: synthesis and speed

After you can solve the above, revisit old problems and optimize:

- write shorter constructions
- use reusable subcircuits
- test more systematically
- recognize when a problem is "really" state prep, oracle conversion, reflection, or arithmetic

## How to use the tracks

Do not solve one problem by luck and move on.

For each problem:

1. write the target transformation in bra-ket or basis-state language
2. identify the chapter pattern it belongs to
3. implement the smallest clean circuit
4. verify with exact simulation
5. only then submit
