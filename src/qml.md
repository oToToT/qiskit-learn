# Quantum Machine Learning With Qiskit

If you want to write quantum machine learning code after this book, you need a realistic starting point, not marketing.

## What QML usually looks like in practice

In Qiskit, beginner-friendly QML work usually falls into two families:

- quantum kernels
- variational quantum models

The relevant package is `qiskit-machine-learning`.

## Install the extra package

If you want to run the package-specific examples in this chapter:

```bash
uv add qiskit-machine-learning
```

The circuit design ideas, however, are mostly plain Qiskit ideas you already know.

## Quantum kernels

The workflow is usually:

1. encode classical data with a feature map
2. compare encoded states through a kernel
3. feed the kernel to a classical learner such as an SVM

This is conceptually close to the circuit skills you already built:

- data encoding is state preparation
- similarity is controlled by phase and basis structure

## A tiny feature-map example

```python
from qiskit import QuantumCircuit
from qiskit.circuit import ParameterVector

x = ParameterVector("x", 2)

feature_map = QuantumCircuit(2)
feature_map.h([0, 1])
feature_map.rz(x[0], 0)
feature_map.rz(x[1], 1)
feature_map.cx(0, 1)
feature_map.rz((x[0] + x[1]), 1)
feature_map.cx(0, 1)

print(feature_map)
```

Do not worry yet about whether this is the best feature map. The point is to see what QML circuits look like: parameterized state-preparation blocks.

## Variational models

A variational model usually has:

- a feature map
- a parameterized ansatz
- an optimizer
- a loss function

The circuit layer is something like this:

```python
from qiskit import QuantumCircuit
from qiskit.circuit import ParameterVector

theta = ParameterVector("theta", 4)

ansatz = QuantumCircuit(2)
ansatz.ry(theta[0], 0)
ansatz.ry(theta[1], 1)
ansatz.cx(0, 1)
ansatz.rz(theta[2], 0)
ansatz.ry(theta[3], 1)

print(ansatz)
```

This is where parameterized circuits and clean Qiskit workflow really matter.

## How the earlier chapters support QML

Kernel methods lean on:

- state preparation
- basis changes
- phase reasoning

Variational methods lean on:

- parameterized gates
- entangling layers
- measurement design
- systematic debugging

So QML is not a separate island. It is built from the same circuit vocabulary you have already been learning.

## A realistic beginner goal

You do not need to become a QML researcher immediately. A good beginner goal is:

- read a kernel tutorial without getting lost in the circuit layer
- write a small parameterized ansatz
- understand where trainable parameters live
- know how to inspect a circuit before wrapping it in a training loop

If you can do that, the official Qiskit Machine Learning tutorials become much more approachable.

## What to watch out for

QML examples can become confusing for reasons that are not specifically "machine learning":

- feature-map parameters versus trainable parameters
- classical preprocessing versus quantum encoding
- shot noise versus exact simulation
- overcomplicated ansatzes that are hard to debug

Keep the circuit part small until the workflow is stable.

## Checkpoint Exercises

1. Install `qiskit-machine-learning` with `uv add qiskit-machine-learning`.
2. Write a tiny parameterized feature map on 2 qubits.
3. Write a simple variational ansatz with a few trainable angles.
4. Explain which earlier chapters in this book support kernel methods and which support variational methods.
