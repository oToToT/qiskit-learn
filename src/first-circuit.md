# Your First Circuit

Let's write your first quantum circuit. By the end of this chapter, you'll have run code that creates superposition—the core quantum phenomenon that makes everything else interesting.

## The Minimal Working Example

Type this code and run it:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create a quantum circuit with 1 qubit
qc = QuantumCircuit(1)

# Apply a Hadamard gate
qc.h(0)

# Print the circuit diagram
print("Circuit:")
print(qc)

# Get and print the quantum state
print("\nQuantum State:")
state = Statevector.from_instruction(qc)
print(state)
```

You should see output like this:

```
Circuit:
    ┌───┐
 q: ┤ H ├
    └───┘

Quantum State:
Statevector([0.70710678+0.j, 0.70710678+0.j],
            dims=(2,))
```

Let's understand what this means.

## Reading the Circuit Diagram

The circuit diagram shows:

- One qubit, labeled `q`
- One gate: `H` (Hadamard)

```
   ┌───┐
q: ┤ H ├
   └───┘
```

## Reading the Statevector

The statevector `[0.70710678+0.j, 0.70710678+0.j]` represents:

\\[|\\psi\\rangle = 0.707\\ldots |0\\rangle + 0.707\\ldots |1\\rangle\\]

In cleaner notation:

\\[|\\psi\\rangle = \\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\]

This is called the **plus state** (or \\(|+\\rangle\\)).

## What Just Happened?

Here's what you need to understand:

1. **Initial state**: Every qubit starts as \\(|0\\rangle\\)
1. **Hadamard gate**: Transforms \\(|0\\rangle\\) into a superposition of \\(|0\\rangle\\) and \\(|1\\rangle\\)
1. **Superposition**: The qubit is *both* \\(|0\\rangle\\) *and* \\(|1\\rangle\\) at the same time (until measured)

The number `0.707...` is \\(\\frac{1}{\\sqrt{2}}\\), which ensures the probabilities add up to 1.

## A Circuit Is a Recipe

Keep this distinction clear:

| Concept | Description | Example |
|---------|-------------|---------|
| **Circuit** | The sequence of gates to apply | `h(0)` |
| **Statevector** | The quantum state produced by the circuit | \\(\\frac{|0\\rangle+|1\\rangle}{\\sqrt{2}}\\) |
| **Measurement** | The process of getting a classical result | `"0"` or `"1"` |

A circuit is like a recipe. Apply the steps, and you get a state. Measure the state, and you get a result.

## Two More Simple Examples

### Example 1: Prepare \\(|1\\rangle\\)

```python
qc = QuantumCircuit(1)
qc.x(0)  # Apply the X (NOT) gate

state = Statevector.from_instruction(qc)
print(state)
```

Output:

```
Statevector([0.+0.j, 1.+0.j],
            dims=(2,))
```

This means: \\(|\\psi\\rangle = 0|0\\rangle + 1|1\\rangle = |1\\rangle\\)

### Example 2: Apply X then H

```python
qc = QuantumCircuit(1)
qc.x(0)    # First: |0> -> |1>
qc.h(0)    # Then: |1> -> ?

state = Statevector.from_instruction(qc)
print(state)
```

What do you think the state will be?

<details>
<summary>Answer</summary>

The state is \\(\\frac{|0\\rangle - |1\\rangle}{\\sqrt{2}}\\), also known as the **minus state** or \\(|-\\rangle\\).

```
Statevector([ 0.70710678+0.j, -0.70710678+0.j],
            dims=(2,))
```

The minus sign on the \\(|1\\rangle\\) coefficient means this state behaves differently from \\(|+\\rangle\\) even though they look similar when measured.

</details>

## Predict Before You Run

This habit is crucial for learning quantum computing. Before running any code:

1. Ask yourself: "What state should this circuit produce?"
1. Run the code
1. Compare your prediction to the actual result

This "predict, then verify" cycle builds intuition faster than anything else.

## The Importance of Inspection

You might wonder: why not just measure and see the result?

Because **measurement destroys quantum information**. When you measure a qubit in superposition, it "collapses" to either \\(|0\\rangle\\) or \\(|1\\rangle\\) randomly. You lose access to the superposition.

By inspecting the statevector *before* measurement, you can see the full quantum state—including phases and amplitudes that measurement can't reveal.

## Your Turn

Try these exercises:

### Exercise 1

Create a circuit that produces the state \\(|0\\rangle\\) (the initial state). Verify with `Statevector`.

### Exercise 2

Create a circuit that produces the state \\(|1\\rangle\\).

### Exercise 3

Predict what `h(0)` followed by `x(0)` produces. Then verify with code.

### Exercise 4

Create two *different* circuits that both produce the state \\(\\frac{|0\\rangle + |1\\rangle}{\\sqrt{2}}\\).

## Summary

In this chapter, you:

- Created your first quantum circuit
- Applied a Hadamard gate to create superposition
- Inspected the statevector to see the exact quantum state
- Learned the difference between a circuit and its resulting state
- Practiced predicting before running

The pattern you'll use throughout this book:

1. Write a circuit
1. Inspect the state
1. Predict what measurement will show
1. Measure to confirm

______________________________________________________________________

## Try These On QCoder

- [QPC001 A1: Generate State |1>](https://www.qcoder.jp/en/contests/QPC001/problems/A1)
- [QPC001 A2: Generate Plus state](https://www.qcoder.jp/en/contests/QPC001/problems/A2)
- [QPC002 B1: Generate State e^(i theta)|0>](https://www.qcoder.jp/en/contests/QPC002/problems/B1)

______________________________________________________________________

*Next: [What Is a Qubit?](./qubit-mental-model.md)*
