---
id: python.closures
type: concept
title: Closures
description: Inner functions that retain references to enclosing bindings via cells.
tags: [python, functions, closures, nested]
prerequisites:
  - python.functions_as_objects
  - python.scope_legb
related:
  - python.decorators
  - python.generators
resource: https://docs.python.org/3/faq/programming.html#what-are-closures
timestamp: 2026-01-01
---

# Summary

A **closure** is a function object that captures variables from an enclosing scope, stored in **cell** objects so they survive after the outer function returns. Closures enable factories, decorators, and callbacks with persistent state without global variables.

# Mental model

When Python compiles a nested function that references enclosing names, it promotes those names to **cells** (`__closure__` tuple on the function object). Each cell holds a reference to the live binding—not a snapshot copy (unless the object itself is immutable and rebound).

```
outer() returns inner function
inner.__closure__ → (cell → x's current object)
```

`nonlocal` writes through to the captured cell. Reading walks the cell to the current object reference.

# Example

```python
def make_multiplier(factor):
    def multiply(n):
        return n * factor
    return multiply

times3 = make_multiplier(3)
times3(10)   # 30 — factor cell still alive
```

```python
def counters():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc
```

# Common mistakes

- Late-binding closures in loops (`funcs = [lambda: i for i in range(3)]` captures one `i`).
- Expecting captured variables to be copies of mutable objects (they are references).
- Using closures when a small class with explicit state would be clearer.
- Forgetting `nonlocal` when assigning to enclosed names (creates a new local instead).

# Related concepts

- [Decorators](/concepts/decorators.md)
- [Functions as Objects](/concepts/functions_as_objects.md)
- [Generators](/concepts/generators.md)
