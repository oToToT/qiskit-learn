# A Practice Ladder

Here is a progression you can follow after finishing the main chapters.

## Level 1: State preparation

Goal: stop hesitating with basis states, `H`, and sign changes.

Practice until you can quickly write circuits for:

- basis states on 1 to 3 qubits
- Bell states
- equal superpositions
- simple sign-flipped variants

## Level 2: Controlled logic

Goal: stop treating `cx`, `cz`, and controlled phase operations as special effects.

Practice until you can:

- explain a control and target clearly
- mark a chosen basis state
- use `x` gates to turn an arbitrary target into an all-ones pattern and back

## Level 3: Interference patterns

Goal: become comfortable with the idea that equal probabilities can still hide different states.

Practice until you can:

- give an example of two states with the same measurement counts
- explain why later gates can distinguish them
- verify your claim with Qiskit

## Level 4: Tiny algorithms

Goal: combine the previous levels into a short end-to-end circuit.

Good mini-projects:

- two-qubit Grover search
- a three-qubit marked-state oracle
- a short notebook comparing statevector inspection and shot-based sampling

## When to move on

Move to larger official tutorials only after you can read these smaller circuits without feeling lost.

That is the point where the official docs become much more useful instead of overwhelming.
