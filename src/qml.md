# Quantum Machine Learning with Qiskit

Quantum Machine Learning (QML) combines quantum computing with classical machine learning. This chapter gives you a realistic starting point.

## What QML Looks Like in Practice

Beginner-friendly QML typically falls into two categories:

| Type | Description | Qiskit Package |
|------|-------------|----------------|
| **Quantum Kernels** | Measure similarity between quantum states | `qiskit-machine-learning` |
| **Variational Models** | Parameterized circuits optimized for tasks | `qiskit-machine-learning` |

## Installing the Package

```bash
uv add qiskit-machine-learning
```

## Quantum Kernels

The workflow:

1. Encode classical data into quantum states (feature map)
1. Prepare states and measure overlap (kernel)
1. Use kernel in classical ML algorithm

```python
from qiskit import QuantumCircuit
from qiskit.circuit import ParameterVector

# Simple feature map
def feature_map(x, n=2):
    qc = QuantumCircuit(n)
    for i in range(n):
        qc.h(i)
        qc.rz(x[i], i)
    qc.cx(0, 1)
    return qc

# Use it
x = ParameterVector("x", 2)
fm = feature_map(x)
print(fm)
```

## Variational Circuits

A variational circuit has:

1. Feature map (encodes data)
1. Ansatz (trainable parameters)
1. Measurement (outputs predictions)

```python
from qiskit import QuantumCircuit
from qiskit.circuit import ParameterVector

# Trainable ansatz
def ansatz(params, n=2):
    qc = QuantumCircuit(n)
    for i in range(n):
        qc.ry(params[i], i)
    qc.cx(0, 1)
    for i in range(n):
        qc.rz(params[n+i], i)
    return qc

# Use it
params = ParameterVector("θ", 4)
ans = ansatz(params)
print(ans)
```

## Connecting to Scikit-Learn

```python
from qiskit_machine_learning.algorithms import QiskitClassifier
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split

# Simple example structure (full implementation requires more setup)
X, y = make_classification(n_samples=100, n_features=2, n_classes=2)
X_train, X_test, y_train, y_test = train_test_split(X, y)
```

## How Earlier Chapters Support QML

| QML Concept | Relevant Chapter |
|-------------|------------------|
| Feature maps | State preparation, basis changes |
| Kernel computation | Phase, interference |
| Ansatz design | Single/two-qubit gates |
| Gradient computation | Measurement, sampling |

QML isn't a separate island—it's built from circuit primitives.

## A Realistic Beginner Goal

You don't need to become a QML researcher. A good goal:

- [ ] Read a kernel tutorial without getting lost
- [ ] Write a parameterized feature map
- [ ] Write a variational ansatz
- [ ] Understand where trainable parameters live
- [ ] Debug a circuit before training

If you can do these, official tutorials become approachable.

## What to Watch Out For

| Pitfall | Symptom |
|---------|---------|
| Too many qubits | Barren plateaus, noise |
| Feature/ansatz confusion | Wrong parameters optimized |
| Classical preprocessing | Data not encoded properly |
| Shot noise | Inconsistent gradients |

## Checkpoint Exercises

### Exercise 1

Install `qiskit-machine-learning`.

### Exercise 2

Write a 2-qubit feature map with parameterized rotations.

### Exercise 3

Write a 4-parameter variational ansatz.

### Exercise 4

Identify which earlier chapters support kernel methods vs variational methods.

______________________________________________________________________

*Next: [Next Steps](./next-steps.md)*
