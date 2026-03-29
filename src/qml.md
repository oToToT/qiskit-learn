# Quantum Machine Learning With Qiskit

If you want to write quantum machine learning code after this book, you need a realistic starting point, not marketing.

## What QML usually looks like in practice

In Qiskit, beginner-friendly QML work usually falls into two families:

- quantum kernels
- variational quantum models

The relevant package is `qiskit-machine-learning`.

## Quantum kernels

The workflow is usually:

1. encode classical data with a feature map
2. compare encoded states through a kernel
3. feed the kernel to a classical learner such as an SVM

This is conceptually close to the circuit skills you already built:

- data encoding is state preparation
- similarity is controlled by phase and basis structure

## Variational models

A variational model usually has:

- a feature map
- a parameterized ansatz
- an optimizer
- a loss function

This is where parameterized circuits and clean Qiskit workflow really matter.

## What you should be able to do after this chapter

You do not need to become a QML researcher immediately. A good beginner goal is:

- read a kernel tutorial without getting lost in the circuit layer
- write a small parameterized ansatz
- understand where training parameters live

## Suggested next reading

The official Qiskit Machine Learning tutorials worth studying next include:

- quantum kernel tutorials
- neural network classifier and regressor tutorials
- quantum kernel training tutorials

## Checkpoint Exercises

1. Install `qiskit-machine-learning` with `uv add qiskit-machine-learning`.
2. Write a tiny parameterized feature map on 2 qubits.
3. Write a simple variational ansatz with a few trainable angles.
4. Explain which earlier chapters in this book support kernel methods and which support variational methods.
