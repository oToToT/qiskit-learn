# Next Steps

You've completed the tutorial. Now what? Here's how to keep growing.

## If You Want to Get Stronger at QCoder

Practice with these contest archives:

| Level | Problems | Focus |
|-------|----------|-------|
| Beginner | QPC001 A-series | State preparation |
| Intermediate | QPC003 A-series | Complex state prep |
| Intermediate | QPC002 B-series | Phase and oracles |
| Advanced | QPC003 B-series | Reflections, Grover |
| Advanced | QPC005 | Fourier basis |

Work through them in order. Each contest builds on the previous.

## If You Want Algorithm Depth

Next topics to study:

- **Quantum Phase Estimation** — Estimate eigenvalues of unitary operators
- **Shor's Algorithm** — Factor integers (requires QFT + modular arithmetic)
- **Quantum Walk Algorithms** — Generalization of Grover
- **Amplitude Estimation** — Quantum version of Monte Carlo

At this point, the official Qiskit tutorials become essential.

## If You Want QML Depth

Next topics to study:

- **Quantum Kernels** — Measure quantum state similarity
- **Variational Quantum Eigensolvers (VQE)** — Find ground states
- **Quantum Approximate Optimization (QAOA)** — Combinatorial optimization
- **Hybrid Classical-Quantum Training** — Gradients, optimizers

## If You Want Hardware Experience

Next steps:

1. Get an IBM Quantum account (free tier available)
1. Run circuits on real quantum hardware
1. Learn about transpilation (circuit rewriting for hardware)
1. Study noise models and error mitigation

## The Standard to Maintain

Don't settle for "I read about it." Hold yourself to:

1. **State the transformation** — Can you write the intended unitary?
1. **Implement it** — Can you write the circuit in Qiskit?
1. **Test it** — Can you verify correctness on small inputs?
1. **Explain it** — Can you explain why it works?

That standard keeps you honest.

## Recommended Resources

| Resource | Type | Level |
|----------|------|-------|
| [Qiskit Documentation](https://docs.quantum.ibm.com/) | Official docs | All levels |
| [Qiskit Textbook](https://learn.qiskit.org/) | Tutorials | Beginner-Intermediate |
| [QCoder](https://www.qcoder.jp/) | Practice problems | All levels |
| [Mike & Ike](https://mikeikebook.org/) | Textbook | Advanced |

## Official API References

For quick reference while working through the chapters:

### Core Qiskit

- [QuantumCircuit API](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.QuantumCircuit)
- [Statevector](https://docs.quantum.ibm.com/api/qiskit/qiskit.quantum_info.Statevector)
- [StatevectorSampler](https://docs.quantum.ibm.com/api/qiskit/qiskit.primitives.StatevectorSampler)
- [Operator](https://docs.quantum.ibm.com/api/qiskit/qiskit.quantum_info.Operator)

### Gates

- [Single-qubit gates](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.UGate)
- [Pauli gates (X, Y, Z)](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.PauliGate)
- [Hadamard gate](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.HGate)
- [CNOT (CX) gate](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.CXGate)
- [CZ gate](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.CZGate)
- [SWAP gate](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.SWAPGate)
- [Rotation gates (RX, RY, RZ)](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.RXGate)
- [Phase gate (P)](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.PhaseGate)

### IBM Learning Courses

- [Basics of Quantum Information](https://learning.quantum.ibm.com/course/basics-of-quantum-information)
- [Fundamentals of Quantum Algorithms](https://learning.quantum.ibm.com/course/fundamentals-of-quantum-algorithms)
- [Introduction to Quantum Superposition](https://learning.quantum.ibm.com/tutorial/introduction-to-quantum-superposition)
- [Entanglement](https://learning.quantum.ibm.com/tutorial/entanglement)
- [Multi-qubit circuits](https://learning.quantum.ibm.com/tutorial/multi-qubit-circuits)
- [Phase kickback](https://learning.quantum.ibm.com/tutorial/phase-kickback-and-phase-rotations)

### Qiskit Machine Learning

- [Qiskit Machine Learning](https://qiskit.github.io/qiskit-machine-learning/)
- [BellState circuit](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.BellGate)

## A Final Thought

Quantum computing is hard. Not hard like magic—hard like any deep technical skill. It rewards patience and systematic learning.

You now have the foundation. The rest is practice.

Build circuits. Break them. Fix them. Submit QCoder problems. Read the docs. Ask questions.

The quantum future needs people who understand this stuff.

Go make it happen.

______________________________________________________________________

*Congratulations on completing Qiskit for Curious Programmers!*
