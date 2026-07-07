---
type: Concept
title: The Iteration Protocol
description: "Iterables expose __iter__; iterators expose __next__ until StopIteration."
tags: [python, iteration, protocol, iterator]
prerequisites:
  - concepts/collections
  - concepts/dunder_methods
related:
  - concepts/generators
  - concepts/data_model_fundamentals
resource: "https://docs.python.org/3/tutorial/classes.html#iterators"
timestamp: 2026-01-01
---

# Summary

**Iteration** in Python is protocol-based. An **iterable** implements `__iter__` returning an **iterator**. An iterator implements `__next__` to produce items or raise `StopIteration`. `for` loops and comprehensions use this protocol under the hood.

# Mental model

```
for x in iterable:
    body
```

Expands to:

1. `it = iter(iterable)` → calls `iterable.__iter__()`
2. Repeatedly `value = next(it)` → `it.__next__()`
3. On `StopIteration`, exit loop

Iterators are **stateful objects**—exhausting them is permanent unless the iterable can produce a fresh iterator (lists can; generators cannot rewind without teeing).

Many built-ins accept arbitrary iterables, not just sequences—duck typing via the protocol.

# Example

```python
class Countdown:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value
```

```python
for ch in "hi":
    print(ch)   # str iterator yields characters
```

# Common mistakes

- Iterating the same iterator twice expecting a full replay.
- Implementing `__getitem__` only (old sequence protocol) instead of `__iter__` for new code.
- Catching `StopIteration` inside generators incorrectly (use `return` in 3.3+ generator semantics).
- Confusing iterable vs iterator (`list` is iterable; `iter(list)` is iterator).

# Related concepts

- [Generators](/concepts/generators.md)
- [Dunder Methods](/concepts/dunder_methods.md)
- [Collections](/concepts/collections.md)
