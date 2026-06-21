---
id: python.dunder_methods
type: concept
title: Dunder Methods
description: Double-underscore hooks customize how objects participate in Python protocols.
tags: [python, oop, dunder, magic-methods]
prerequisites:
  - python.classes
related:
  - python.data_model_fundamentals
  - python.iteration_protocol
  - python.object_identity_equality
resource: https://docs.python.org/3/reference/datamodel.html#special-method-names
timestamp: 2026-01-01
---

# Summary

**Dunder** (double underscore) methods like `__str__`, `__repr__`, `__eq__`, and `__getitem__` define how objects interact with operators, builtins, and protocols. They are called by the interpreter—not as casual public API—and enable Python's uniform object syntax.

# Mental model

Syntax sugar maps to dunder calls:

| Syntax | Invokes |
|--------|---------|
| `str(obj)` | `obj.__str__()` |
| `repr(obj)` | `obj.__repr__()` |
| `a == b` | `a.__eq__(b)` |
| `len(obj)` | `obj.__len__()` |
| `obj[key]` | `obj.__getitem__(key)` |

Descriptors (`__get__`, `__set__`) power properties and bound methods. Container, numeric, and context manager protocols each have defined dunder sets.

Implement the minimal set that makes your type feel native; lean on defaults from `object` when appropriate.

# Example

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
```

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    def __str__(self):
        return f"{self.celsius}°C"
```

# Common mistakes

- Implementing `__eq__` without considering `__hash__` for use in sets/dicts.
- Using `__init__` for expensive work that belongs in `__new__` or factories.
- Calling dunder methods directly (`obj.__len__()`) instead of builtins (`len(obj)`).
- Returning `NotImplemented` vs raising `TypeError` incorrectly in binary ops.

# Related concepts

- [Data Model Fundamentals](/concepts/data_model_fundamentals.md)
- [The Iteration Protocol](/concepts/iteration_protocol.md)
- [Object Identity vs Equality](/concepts/object_identity_equality.md)
