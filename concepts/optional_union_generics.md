---
id: python.optional_union_generics
type: concept
title: Optional, Union, and Generics
description: Express nullable, alternates, and parameterized types for checkers.
tags: [python, typing, union, optional, generics]
prerequisites:
  - python.typing_module
related:
  - python.type_hints
  - python.classes
resource: https://docs.python.org/3/library/typing.html#typing.Union
timestamp: 2026-01-01
---

# Summary

**Unions** express values that may be one of several types (`int | str` or `Union[int, str]`). **Optional[T]** means `T | None`. **Generics** parameterize classes and functions (`list[T]`, `TypeVar`) so checkers track element types through containers and APIs.

# Mental model

```
Optional[str]  ≡  str | None
Union[A, B]    ≡  A | B   (3.10+ prefer | syntax)

def first(items: list[T]) -> T: ...
```

`TypeVar` can be bounded (`TypeVar("T", bound=Base)`) or constrained to a set of types. `Generic` base on classes tells checkers how type parameters flow through methods.

At runtime, `list[int]` is equivalent to plain `list`—generic parameters are erased for execution.

# Example

```python
from typing import TypeVar

T = TypeVar("T")

def maybe_double(value: int | None) -> int | None:
    if value is None:
        return None
    return value * 2
```

```python
class Cache(Generic[K, V]):
    def __init__(self) -> None:
        self._data: dict[K, V] = {}

    def set(self, key: K, value: V) -> None:
        self._data[key] = value
```

# Common mistakes

- Using `Optional[str]` when `str` alone was intended (implicitly required).
- Creating overly wide unions instead of narrowing with `isinstance` or match.
- Expecting generics to enforce container element types at runtime.
- Forgetting that `Union[int, None]` still needs explicit `None` checks for checkers after calls.

# Related concepts

- [The typing Module](/concepts/typing_module.md)
- [Type Hints](/concepts/type_hints.md)
- [Classes](/concepts/classes.md)
