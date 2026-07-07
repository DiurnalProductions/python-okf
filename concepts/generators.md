---
type: Concept
title: Generators
description: "Generator functions suspend execution, yielding values via frozen frames."
tags: [python, generators, iteration, lazy]
prerequisites:
  - concepts/iteration_protocol
  - concepts/functions_as_objects
related:
  - concepts/closures
  - concepts/coroutines
resource: "https://docs.python.org/3/tutorial/classes.html#generators"
timestamp: 2026-01-01
---

# Summary

A **generator** is an iterator produced by a generator function (`yield`) or generator expression. Each `yield` suspends the function's frame, preserving local state. Resuming continues after the yield. Generators enable lazy, memory-efficient iteration pipelines.

# Mental model

`def gen(): yield x` compiles to a function that returns a generator object on call—**the body does not run until iteration begins**.

```
call gen() → generator object (iterator)
next()   → run until yield → pause frame
next()   → resume → yield again or StopIteration
```

Generator objects hold references to their stack frame and local bindings—similar machinery later powers async coroutines (distinct but related).

`yield from` delegates to sub-iterables or sub-generators, forwarding sends and throws in advanced patterns.

# Example

```python
def read_chunks(path, size=1024):
    with open(path, "rb") as f:
        while chunk := f.read(size):
            yield chunk
```

```python
squares = (n * n for n in range(10))   # generator expression
list(squares)   # materialize once
```

# Common mistakes

- Calling a generator function expecting immediate computation.
- Assuming generators can be rewound without `itertools.tee` or re-instantiation.
- Mixing `return value` in generators (becomes `StopIteration.value` in 3.3+).
- Using generators when eager lists are simpler and small enough to fit in memory.

# Related concepts

- [The Iteration Protocol](/concepts/iteration_protocol.md)
- [Coroutines](/concepts/coroutines.md)
- [Closures](/concepts/closures.md)
