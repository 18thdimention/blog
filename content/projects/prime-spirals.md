---
title: Prime Spirals
date: 2025-01-15
description: An interactive visualization of prime number distribution on Ulam spirals, built with WebGL and Rust. Explore how primes cluster along unexpected diagonal lines when integers are arranged in a spiral pattern.
---

## Jan 2025 ~ Feb 2026

An interactive visualization of prime number distribution on Ulam spirals, built with WebGL and Rust.

### Motivation

In 1963, Stanislaw Ulam was doodling during a lecture. He wrote integers in a spiral and circled the primes — and noticed they clustered along diagonal lines. This seemingly trivial observation connects to some of the deepest unsolved problems in number theory.

I wanted to make this accessible. Not as a static image, but as something you could explore — zoom into, rotate, color by different properties.

### Technical approach

The backend is written in Rust for performance. Generating primes up to 10 million and computing their spiral coordinates needs to be fast. The frontend uses WebGL for rendering millions of points without lag.

The color mapping is customizable: you can color primes by their distance from the nearest twin prime, by their position in the Fibonacci sequence, or by their remainder modulo small numbers. Each coloring reveals different patterns.

### What I learned

Prime distribution is stranger than I expected. The diagonal lines in the Ulam spiral correspond to quadratic polynomials that produce unusually many primes — a fact that connects to the Bunyakovsky conjecture, still unproven.

Building this also taught me that mathematical visualization is a form of translation, much like writing. You're taking something abstract and making it legible.

![Ulam spiral visualization](https://picsum.photos/seed/prime1/400/300)
![Color-mapped primes](https://picsum.photos/seed/prime2/400/300)
![Diagonal patterns](https://picsum.photos/seed/prime3/400/300)
