# Introduction

## Welcome to Quantum Computing

You've probably heard that quantum computers are magical machines that solve impossible problems. Maybe you've seen headlines about "quantum supremacy" or watched videos of people getting excited about qubits.

Here's what those headlines don't tell you: quantum computing is also *hard*. Not hard like building a spaceship, but hard like learning a new way of thinking. The mathematics is real. The intuition takes time.

**This book is for programmers who want to build that intuition from scratch.**

## What This Book Is

This is a tutorial written for curious programmers who are comfortable with Python and want to understand quantum computing through hands-on practice with Qiskit.

Think of it like:

- **The Rust Book** — one concept at a time, with working examples
- **A language learning app** — daily practice with QCoder problems
- **A lab manual** — not a novel to read passively

The official Qiskit documentation is excellent. It is also overwhelming. This book strips away the noise and builds up your understanding step by step, so that when you *do* read the official docs, everything clicks.

## What You'll Learn

By the end of this book, you will be able to:

| Skill | What It Means |
|-------|---------------|
| **Read circuits** | Understand what a circuit diagram means before running anything |
| **Write circuits** | Build circuits that do exactly what you intend |
| **Debug with statevectors** | Inspect exact quantum states to find bugs |
| **Design patterns** | Recognize state preparation, oracles, and reflections |
| **Solve QCoder problems** | Work through the QCoder problem sets with confidence |
| **Explore QML** | Read quantum machine learning code without getting lost |

This is a realistic goal. It is also a strong one.

## The Teaching Philosophy

This book is built on a few core beliefs about how people actually learn difficult technical subjects.

### Circuits Before Hardware

You will first understand what a circuit *does mathematically*. Hardware noise, transpilation, calibration, and backend management are real concerns—but they are distractions if you still confuse state preparation with measurement.

We will focus entirely on circuit behavior using exact simulation.

### Exact Before Noisy

Before asking "what outcomes appear?", we ask "what state did I create?"

This means we use `Statevector` and `StatevectorSampler` extensively. Not because sampling isn't important, but because the exact state reveals the *why* behind the numbers.

### Patterns Before Prestige Algorithms

It is better to deeply understand:

- how to prepare basis states
- how superposition works
- what entanglement means in practice
- how oracles and reflections work

...than to vaguely memorize Grover's algorithm or the Quantum Fourier Transform. Those bigger topics become *obviously useful* once the patterns are solid.

### Practice Before Theory

Every chapter includes:

1. A mental model to understand conceptually
1. Working Qiskit code to try immediately
1. Exercises to build muscle memory
1. QCoder problems to test your understanding

Reading without practicing is like watching swimming videos and expecting to know how to swim.

## Why Qiskit?

Qiskit is IBM's open-source quantum computing framework. It is:

- **Well-documented** — official tutorials are extensive
- **Actively maintained** — regular updates and improvements
- **Beginner-friendly APIs** — the circuit interface is clean
- **Industry-standard** — used in real quantum research

If you learn Qiskit well, you can contribute to real quantum computing projects.

> **Official Resources:** The official Qiskit documentation is at [docs.quantum.ibm.com](https://docs.quantum.ibm.com/). IBM also offers structured learning courses including [Basics of Quantum Information](https://learning.quantum.ibm.com/course/basics-of-quantum-information) and [Fundamentals of Quantum Algorithms](https://learning.quantum.ibm.com/course/fundamentals-of-quantum-algorithms). This tutorial complements those resources by providing a circuit-first, problem-driven approach.

## Why QCoder?

QCoder is a Japanese quantum programming contest platform that presents quantum problems as concrete circuit challenges. Each problem tells you exactly what the input and output should be, and you write the circuit to make it happen.

**Why is this valuable?**

Because quantum computing has a reputation for feeling "mysterious." QCoder forces you to turn vague understanding into circuits that actually work. If you can solve QCoder problems, you understand the material.

## What This Book Is NOT

This book does not promise to:

- Make you a quantum algorithm researcher overnight
- Replace the official Qiskit documentation
- Cover every gate, every algorithm, every hardware consideration
- Teach quantum physics in depth

What it *does* do is bring you to the point where:

- Official tutorials feel approachable
- QCoder problems look structured instead of mysterious
- You can design and test your own circuits confidently

## Before You Begin

Make sure you:

1. Are comfortable with Python (functions, classes, loops)
1. Have basic linear algebra (vectors, matrices, multiplication) — or are willing to learn
1. Have set up your environment (see [Setup](./setup.md))
1. Are ready to practice, not just read

Quantum computing rewards patience. Take your time with the early chapters—they are the foundation for everything that comes after.

Let's begin.

______________________________________________________________________

*Next: [How To Use This Book](./how-to-use-this-book.md)*
