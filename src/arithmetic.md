# Arithmetic And Modular Thinking

A surprising number of contest problems stop being scary once you reframe them as reversible arithmetic.

## Why arithmetic matters

Arithmetic circuits show up in:

- modular algorithms
- phase-estimation-style constructions
- hidden-number tasks
- more advanced QCoder problems

At first, you learn gates.

Then you learn state preparation and oracles.

Then you learn that many harder circuits are really structured data movement:

- add a constant
- shift a register
- swap positions
- apply a permutation reversibly

## Start with basis-state mappings

Before you write any gates, write the intended action on basis states.

Examples:

- `|x, y> -> |y, x>` for a swap
- `|x> -> |x + 1 mod 4>` for a cyclic shift on two qubits
- `|x, 0> -> |x, f(x)>` for a reversible compute step

This habit is more important than any individual gate trick.

## Example 1: swap

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(2)
qc.swap(0, 1)
print(qc)
```

This is a tiny arithmetic circuit because it permutes basis states reversibly.

## Example 2: exhaustive testing on small registers

For arithmetic-like circuits, exhaustive testing is often possible and should be routine.

```python
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

def basis_state_circuit(bits: str) -> QuantumCircuit:
    qc = QuantumCircuit(len(bits))
    for i, bit in enumerate(reversed(bits)):
        if bit == "1":
            qc.x(i)
    return qc

swap = QuantumCircuit(2)
swap.swap(0, 1)

for bits in ["00", "01", "10", "11"]:
    qc = basis_state_circuit(bits)
    qc.compose(swap, inplace=True)
    print(bits, "->", Statevector.from_instruction(qc))
```

This is exactly the kind of harness you should build for small arithmetic and oracle circuits.

## Example 3: cyclic shift as a permutation

A cyclic shift problem is often easier to understand if you forget "quantum" for a moment and ask:

which basis state should each input basis state map to?

If you can answer that cleanly, you can often synthesize the circuit using swaps, `x` gates, controls, or Fourier-basis tricks depending on the problem statement.

## Modular thinking

Many quantum arithmetic tasks are modular by default. Registers have finite size, so operations wrap around.

For a two-qubit register:

- `3 + 1 mod 4 = 0`
- `0 - 1 mod 4 = 3`

If you forget the modulus, your intended mapping will not even be reversible.

## The design checklist

When building arithmetic circuits:

1. define the register layout explicitly
2. state the intended mapping on basis states
3. test small inputs exhaustively
4. uncompute any temporary work registers
5. only then optimize the circuit

This is much more reliable than guessing from circuit diagrams.

## Arithmetic and QFT are connected

Some problems are easiest in the computational basis.

Others become cleaner in the Fourier basis because shifts and additions can become phase operations. That is why the QFT chapter sits nearby in the book. The real subject is not "another algorithm." It is another way of representing the same transformation.

## Checkpoint Exercises

1. Implement a 2-qubit swap and verify it on all basis states.
2. Implement a cyclic shift on a small register.
3. Define a reversible map for adding a known constant to a tiny register.
4. Write tests that compare the output basis state for every input.

## Try These On QCoder

- [QPC002 B3, SWAP Qubits](https://www.qcoder.jp/en/contests/QPC002/problems/B3)
- [QPC002 B5 through B8](https://www.qcoder.jp/en/contests/QPC002)
- [QPC005 B2 and later Fourier-basis arithmetic problems](https://www.qcoder.jp/en/contests/QPC005)
