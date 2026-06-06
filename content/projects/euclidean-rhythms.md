---
title: Euclidean Rhythms
date: 2025-05-20
description: A generative music tool that creates rhythmic patterns using Bjorklund's algorithm, connecting number theory to West African and Latin American percussion traditions. Built with the Web Audio API.
---

## May 2025 ~ Present

A generative music tool that creates rhythmic patterns using Bjorklund's algorithm.

### Motivation

Bjorklund's algorithm distributes k pulses as evenly as possible across n steps — the same problem Euclid solved for computing GCDs. It turns out that nearly every traditional rhythm in West African and Latin American music corresponds to a Euclidean rhythm for some (k, n).

I wanted to build a tool that made this connection audible and playable.

### Technical approach

The interface lets you set k and n and hear the resulting pattern layered over a selectable set of timbres. Multiple layers can be combined, each with different (k, n) values. The underlying synth engine uses the Web Audio API with custom oscillators shaped by wavelet transforms.

A "discover" mode randomly samples Euclidean rhythms and names the traditional pattern they correspond to — tresillo, son clave, bembé — when one exists.

### What I learned

Mathematics and music share more than metaphor. The reason Euclidean rhythms sound good is related to why rational approximations of irrational numbers are useful: even spacing is a kind of fairness, and fairness sounds balanced.

![Rhythm interface](https://picsum.photos/seed/rhythm1/400/300)
![Pattern visualization](https://picsum.photos/seed/rhythm2/400/300)
![Waveform display](https://picsum.photos/seed/rhythm3/400/300)
