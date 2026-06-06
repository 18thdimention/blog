---
title: Topology of Narrative
date: 2024-06-10
description: A digital essay mapping the branching structures of short fiction using tools from algebraic topology. Each story is rendered as a simplicial complex, revealing hidden symmetries in how narratives diverge and converge.
---

## Jun 2024 ~ Dec 2024

A digital essay mapping the branching structures of short fiction using tools from algebraic topology.

### Motivation

I kept noticing that the stories I admired most had a particular shape — not linear, not even simply branching, but something more intricate. I wondered whether the language of topology could make that shape precise.

### Approach

I selected twelve short stories and encoded their narrative graphs: each scene is a vertex, each transition an edge. From these graphs I built simplicial complexes and computed their homology groups. The Betti numbers — counts of connected components, loops, and voids — turned out to correlate with qualities readers describe as "layered" or "resonant."

The essay itself is written as a web piece, with interactive diagrams built in D3.js that let you explore each story's topology alongside the text.

### What I learned

Some stories that feel simple have surprisingly complex topology, and vice versa. Borges, unsurprisingly, maximizes loops. But Alice Munro's seemingly straightforward narratives contain voids — higher-dimensional holes that correspond to things deliberately left unsaid.

![Simplicial complex diagram](https://picsum.photos/seed/topo1/400/300)
![Narrative graph](https://picsum.photos/seed/topo2/400/300)
![Betti number visualization](https://picsum.photos/seed/topo3/400/300)
