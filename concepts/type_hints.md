---
id: python.type_hints
type: concept
title: Type Hints
description: Annotations document expected types without changing runtime behavior.
tags: [python, typing, annotations, static-analysis]
prerequisites:
  - python.dynamic_typing
  - python.functions_as_objects
related:
  - python.typing_module
  - python.runtime_vs_static_typing
resource: https://docs.python.org/3/library/typing.html
timestamp: 2026-01-01
---

# Summary

**Type hints** are expressions attached to parameters, return values, and variables using annotation syntax. They are stored in `__annotations__` and evaluated (with postponed evaluation in modern code) but **not enforced** by the interpreter at runtime unless you use a separate checker or library.

# Mental model

```python
def greet(name: str) -> str:
    return "hi " + name
```

Runtime sees a normal function; `greet.__annotations__` holds `{'name': str, 'return': str}`. Tools like mypy, pyright, and PyCharm read source + stubs to verify consistency **before** execution.

Python 3.10+ allows `X | Y` union syntax; 3.9+ supports built-in generics (`list[int]`).

`from __future__ import annotations` (default in 3.14+) stores annotations as strings, easing forward references.

# Example

```python
def total(prices: list[float], tax: float = 0.0) -> float:
    return sum(prices) * (1 + tax)
```

```python
class User:
    id: int
    name: str

    def __init__(self, id: int, name: str) -> None:
        self.id = id
        self.name = name
```

# Common mistakes

- Believing annotations prevent runtime type errors.
- Using mutable values as defaults in annotations incorrectly (defaults are values, annotations are types).
- Over-annotating trivial scripts where tests document behavior better.
- Confusing `Optional[T]` requirement with parameters that accept `None` at runtime without checks.

# Related concepts

- [The typing Module](/concepts/typing_module.md)
- [Runtime vs Static Typing](/concepts/runtime_vs_static_typing.md)
- [Dynamic Typing](/concepts/dynamic_typing.md)
