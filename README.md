# Qiskit for Curious Programmers

A beginner-friendly Qiskit tutorial written with `mdBook`.

## Local development

Install Python dependencies:

```bash
uv sync
```

Build the book:

```bash
mdbook build
```

Serve the book locally:

```bash
mdbook serve --open
```

## Tutorial shape

The book is intentionally not a full reference.

It focuses on:

- small circuits first
- statevector inspection before noisy sampling
- QCoder-style drills for practice
- enough intuition to make the official Qiskit docs easier to approach later
