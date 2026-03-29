# The Single-Qubit Toolbox

Most early QCoder problems are really asking one question:

"Can you control one qubit well?"

That sounds small, but one-qubit fluency carries a lot of the book.

## The first gates to master

- `x`: swaps `|0>` and `|1>`
- `z`: flips the sign of `|1>`
- `h`: changes between the computational basis and the plus/minus basis
- `rx`, `ry`, `rz`: continuous rotations
- `p`: adds a phase to `|1>`

You do not need every gate in the library yet. You do need to know what these do without hesitation.

## `ry` as a state-preparation tool

For real amplitudes, `ry(theta)` is the cleanest gate to learn first:

\\[ R_y(\theta)|0\rangle = \cos(\theta/2)|0\rangle + \sin(\theta/2)|1\rangle \\]

This equation matters because it lets you design a target probability instead of guessing.

For example, if you want probability `3/4` on `|1>`, then you want:

\\[ \sin^2(\theta/2) = 3/4 \\]

so one valid choice is `theta = 2 * asin(sqrt(3) / 2) = 2pi/3`.

```python
from math import pi
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

qc = QuantumCircuit(1)
qc.ry(2 * pi / 3, 0)
print(Statevector.from_instruction(qc))
```

## Why `z` feels useless until it does not

If your qubit is definitely `|0>`, then `z` appears to do nothing.

But after a Hadamard, `z` changes relative phase:

\\[ Z\frac{|0\rangle + |1\rangle}{\sqrt{2}} = \frac{|0\rangle - |1\rangle}{\sqrt{2}} \\]

That is enough to change later interference.

## The plus/minus basis is not optional

The states

\\[ |+\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}}, \qquad |-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} \\]

show up constantly:

- `|+>` is what `h` creates from `|0>`
- `|->` is what `h` creates from `|1>`
- `|->` is the standard ancilla state for phase kickback tricks

You do not need to worship this notation. You do need to recognize it instantly.

## Learn one tiny identity well

\\[ H Z H = X \\]

This is not just algebra. It is the smallest example of a powerful idea:

1. phase information exists in one basis
2. change basis
3. the same action looks like a bit flip

You can check it directly:

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Operator

lhs_circuit = QuantumCircuit(1)
lhs_circuit.h(0)
lhs_circuit.z(0)
lhs_circuit.h(0)

rhs_circuit = QuantumCircuit(1)
rhs_circuit.x(0)

lhs = Operator(lhs_circuit)
rhs = Operator(rhs_circuit)
print(lhs == rhs)
```

If you prefer not to rely on operator equality, test both circuits on `|0>` and `|1>` with `Statevector`.

## Relative phase versus global phase

This distinction becomes important very early.

- multiplying the entire state by `-1` does not change physical behavior
- changing only one branch by `-1` does change interference

So `-|1>` and `|1>` are physically equivalent, but `(|0> + |1>)/sqrt(2)` and `(|0> - |1>)/sqrt(2)` are not.

## Checkpoint Exercises

1. Prepare the minus state.
2. Prepare a state with `P(1)=3/4`.
3. Verify `H Z H = X` on `|0>` and `|1>`.
4. Build three different circuits for `-|1>` and confirm they differ only by global phase.

## Try These On QCoder

- [QPC001 A3, Generate Minus state](https://www.qcoder.jp/en/contests/QPC001/problems/A3)
- [QPC002 B1, Generate State e^(i theta)|0>](https://www.qcoder.jp/en/contests/QPC002/problems/B1)
- [QPC003 B3, Generate State tensor_i (cos T_i |0> + sin T_i |1>)](https://www.qcoder.jp/en/contests/QPC003/problems/B3)
