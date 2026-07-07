---
type: Concept
title: The typing Module
description: Composable type constructors for static analysis and documentation.
tags: [python, typing, generics, protocols]
prerequisites:
  - concepts/type_hints
related:
  - concepts/optional_union_generics
  - concepts/runtime_vs_static_typing
resource: "https://docs.python.org/3/library/typing.html"
timestamp: 2026-07-06
---

# Summary

The **`typing`** module supplies types that do not always exist as runtime classes in older versions: `Protocol`, `TypedDict`, `TypeVar`, `Callable`, `Literal`, `Final`, and more. These exist primarily for **type checkers**, though some have runtime behavior (e.g., `TypedDict` is a dict subclass helper). Python 3.12 (PEP 695) added native generic syntax — `def first[T](items: list[T]) -> T` and `class Stack[T]:` — which replaces most explicit `TypeVar` declarations in new code.

# Mental model

Categories:

- **Aliases** — `Vector = list[float]`
- **Generics** — `class Stack(Generic[T])`
- **Structural types** — `Protocol` defines required methods without inheritance
- **Special forms** — `Union`, `Optional` (legacy), `Annotated`

Checkers interpret these; at runtime many are identity functions or lightweight placeholders. Prefer modern syntax (`list[int]`, `X | None`) over legacy `List`, `Optional` when targeting 3.10+.

# Example

```python
from typing import Protocol, TypeVar, Generic

T = TypeVar("T")

class Repository(Protocol):
    def get(self, id: int) -> T: ...

class Box(Generic[T]):
  def __init__(self, value: T) -> None:
      self.value = value
```

```python
from typing import TypedDict

class UserRow(TypedDict):
    id: int
    email: str
```

# Common mistakes

- Importing `List`, `Dict` in new 3.9+ code when built-in generics suffice.
- Using `typing` constructs expecting runtime validation by default.
- Defining `Protocol` classes with unnecessary `@runtime_checkable` for `isinstance`.
- Circular import issues from evaluating annotations too early (use `from __future__ import annotations`).

# Related concepts

- [Type Hints](/concepts/type_hints.md)
- [Optional, Union, and Generics](/concepts/optional_union_generics.md)
- [Runtime vs Static Typing](/concepts/runtime_vs_static_typing.md)
