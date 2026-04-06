# A Productive Qiskit Workflow

Writing circuits is half the job. The other half is debugging, testing, and structuring them. This chapter gives you a workflow that scales.

## The Core Loop

For every circuit you build:

1. Write the circuit constructor
1. Inspect `Statevector()`
1. Predict the state
1. Compare actual vs predicted
1. If wrong: debug using the checklist
1. Then: measure and sample

## Write Functions, Not Scripts

```python
from qiskit import QuantumCircuit

def bell_state() -> QuantumCircuit:
    """Create a Bell state."""
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    return qc

def bell_minus() -> QuantumCircuit:
    """Create the |Φ⁻⟩ state."""
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    qc.z(0)
    return qc
```

Benefits:

- Testable
- Composable
- Documented
- Reusable

## Use Statevector Aggressively

```python
from qiskit.quantum_info import Statevector

state = Statevector(bell_state())
print("Probabilities:", state.probabilities())
print("State:", state)
```

Before measurement, always check the statevector. It tells you exactly what's happening.

> **Tip:** For the complete debugging checklist, see [How To Use This Book](./how-to-use-this-book.md#the-debugging-checklist).

## Building Test Harnesses

For arithmetic and oracle circuits:

```python
def test_basis_state_mapping(circuit, n_qubits, expected):
    """Test a circuit on all basis states."""
    results = {}
    for i in range(2**n_qubits):
        # Prepare input state
        qc = QuantumCircuit(n_qubits)
        for j in range(n_qubits):
            if (i >> j) & 1:
                qc.x(j)
        
        # Apply circuit
        qc.compose(circuit, inplace=True)
        
        # Check output
        state = Statevector(qc)
        results[i] = state
    
    return results
```

## Use Parameters

```python
from qiskit.circuit import Parameter

def rotation_circuit(theta):
    qc = QuantumCircuit(1)
    qc.ry(theta, 0)
    return qc

theta = Parameter("θ")
qc = rotation_circuit(theta)

# Evaluate for different values
for val in [0, pi/4, pi/2, pi]:
    assigned = qc.assign_parameters({theta: val})
    print(f"θ={val}: {Statevector(assigned)}")
```

## Compose Large Circuits

```python
# Build subcircuits
oracle = phase_oracle_11()
diffusion = diffusion_2qubit()

# Compose into Grover iteration
grover_iter = QuantumCircuit(2)
grover_iter.compose(oracle, inplace=True)
grover_iter.compose(diffusion, inplace=True)
```

## Inverse and Control

```python
# Inverse of a circuit
circuit_inverse = circuit.inverse()

# Controlled version
controlled_circuit = circuit.control()

# Both
controlled_inverse = circuit.inverse().control()
```

## Save and Load Circuits

```python
# Save circuit
qc.qasm()  # Returns OpenQASM string

# Load circuit
from qiskit import qasm2
qc = qasm2.load("circuit.qasm")
```

## Summary Checklist

- [ ] Write circuit constructors as functions
- [ ] Inspect statevector before measurement
- [ ] Build test harnesses for small circuits
- [ ] Use parameters for reusable circuits
- [ ] Compose subcircuits instead of writing monolithic circuits
- [ ] Keep the debugging checklist in mind
- [ ] Document what each circuit does

## Checkpoint Exercises

### Exercise 1

Refactor a previous exercise into a function with tests.

### Exercise 2

Build a test harness that checks all 2-qubit basis inputs.

### Exercise 3

Parameterize a one-qubit rotation circuit.

### Exercise 4

Build a prepare-reflect-unprepare helper.

______________________________________________________________________

*Next: [Quantum Machine Learning with Qiskit](./qml.md)*
