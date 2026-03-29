# Next Steps

When you finish this book, do not immediately jump to the hardest paper or the largest tutorial.

Pick a lane and keep your practice deliberate.

## If you want to become strong at QCoder

Repeat this loop:

1. classify the problem family
2. write the intended basis-state or phase action
3. implement the smallest correct circuit
4. test with exact simulation first
5. only then optimize or submit

Then work through contest archives in order instead of randomly.

A useful progression is:

- QPC001 and the `A` problems in QPC003 for state preparation
- QPC002 B2 and QPC003 B2/Ex1 for oracle conversion and phase kickback
- QPC003 B4 to B8 for reflections and Grover
- QPC002 B4 to B8 and QPC005 for QFT and arithmetic thinking

## If you want algorithm depth

Study next:

- phase estimation
- Hamiltonian simulation
- amplitude estimation
- fault-tolerant algorithm building blocks

At that point, QFT and reversible arithmetic stop being isolated chapters and start becoming reusable subroutines.

## If you want QML depth

Study next:

- richer feature maps
- trainable kernels
- variational classifiers and regressors
- hybrid integrations with classical ML frameworks

Do not skip the workflow discipline from this book. QML code gets messy very quickly if the circuit layer is not under control.

## A practical standard

The useful goal is not "I read a quantum book."

It is:

- I can state the intended transformation
- I can implement it in Qiskit
- I can test it
- I can explain why it works

That standard is enough to keep improving after the book ends.
