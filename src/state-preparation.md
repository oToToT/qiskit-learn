# State Preparation Patterns

QCoder problems often look like this:

"Create a circuit that prepares the target state."

That is not a separate topic from learning gates. It is where the gates become useful.

## Pattern 1: Basis state preparation

To prepare `|101>`, just flip the right qubits.

```python
qc = QuantumCircuit(3)
qc.x(0)
qc.x(2)
```

## Pattern 2: Uniform superposition

To create equal weight over all `n`-qubit basis states:

```python
qc = QuantumCircuit(n)
for i in range(n):
    qc.h(i)
```

This is the starting point for many search-style algorithms.

## Pattern 3: One branch first, then control the rest

Many two-qubit and three-qubit target states can be built by:

1. setting amplitudes on one qubit
2. using controlled gates to route amplitude into the right basis states

That is more reliable than guessing.

## Pattern 4: Prepare, then fix phases

Sometimes the probabilities already look right, but the signs are wrong.

That is when `z`, `cz`, or `rz` enter the picture.

For example, these states have the same measurement distribution:

\[
\frac{|00\rangle + |11\rangle}{\sqrt{2}}, \quad
\frac{|00\rangle - |11\rangle}{\sqrt{2}}
\]

But they are different states, and future interference can distinguish them.

## DIY

Try to prepare:

1. \((|00\rangle + |11\rangle)/\sqrt{2}\)
2. \((|00\rangle - |11\rangle)/\sqrt{2}\)
3. a three-qubit uniform superposition
4. a state with support only on `|000>` and `|111>`

Write down your plan before writing the code.
