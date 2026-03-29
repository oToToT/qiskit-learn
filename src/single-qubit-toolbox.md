# The Single-Qubit Toolbox

Most early QCoder problems are really asking one question:

"Can you control one qubit well?"

## The first gates to master

- `x`: swaps `|0>` and `|1>`
- `z`: flips the sign of `|1>`
- `h`: changes between the computational basis and the plus/minus basis
- `rx`, `ry`, `rz`: continuous rotations

## `ry` as a state-preparation tool

For real amplitudes, `ry(theta)` is the cleanest gate to learn first:

$$
R_y(\theta)|0\rangle
=
\cos(\theta/2)|0\rangle + \sin(\theta/2)|1\rangle
$$

```python
from math import pi
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.ry(pi / 3, 0)
print(Statevector.from_instruction(qc))
```

## Why `z` feels useless until it does not

If your qubit is definitely `|0>`, then `z` appears to do nothing.

But after a Hadamard, `z` changes relative phase:

$$
Z\frac{|0\rangle + |1\rangle}{\sqrt{2}}
=
\frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

That is enough to change later interference.

## Learn one tiny identity well

$$
H Z H = X
$$

This is not just algebra. It is the simplest example of turning phase information into bit information by changing basis.

## Checkpoint Exercises

1. Prepare the minus state.
2. Prepare a state with `P(1)=3/4`.
3. Verify `H Z H = X` on `|0>` and `|1>`.
4. Build three different circuits for `-|1>` and confirm they differ only by global phase.

## QCoder Connections

- QPC001 A3, "Generate Minus state"
- QPC002 B1, "Generate State e^(i theta)|0>"
- QPC003 B3, "Generate State tensor_i (cos T_i |0> + sin T_i |1>)"
