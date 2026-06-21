---
id: python.dynamic_typing
type: concept
title: Dynamic Typing
description: Object types are determined at runtime; names carry no static type.
tags: [python, typing, runtime, dynamic]
prerequisites:
  - python.variables
  - python.references
related:
  - python.type_hints
  - python.runtime_vs_static_typing
resource: https://docs.python.org/3/glossary.html#term-dynamic-typing
timestamp: 2026-01-01
---

# Summary

Python is **dynamically typed**: the type of a value is a property of the **object** at runtime, not the name that references it. A single name can be rebound to objects of different types over its lifetime. Operations dispatch on the object's type when they execute.

# Mental model

At runtime, every object carries a pointer to its class (`type(obj)`). When you call `len(x)`, Python looks up `x.__class__.__len__` (via the data model) at call time—not at compile time.

```
name "value" → object → class → method table
```

This is **strong** typing in the sense that `3 + "hi"` raises `TypeError` rather than silently coercing. It is **dynamic** because no name is locked to a type unless external tools enforce annotations.

Type hints are optional metadata for humans and static analyzers; they do not change this runtime dispatch unless you use runtime checking libraries.

# Example

```python
x = 42          # x → int
x = "hello"     # x → str (rebinding is legal)
x = [1, 2]      # x → list

len(x)          # dispatches to list.__len__ at runtime
```

```python
def process(item):
    return item.upper()   # fails at runtime if item has no .upper()
```

# Common mistakes

- Believing type hints make Python statically typed at runtime.
- Using `type(x) == list` instead of `isinstance(x, list)` (breaks subclasses).
- Assuming duck typing means "anything works"—missing methods still fail at runtime.
- Confusing dynamic typing with weak typing; Python does not silently convert incompatible types.

# Related concepts

- [Type Hints](/concepts/type_hints.md)
- [Runtime vs Static Typing](/concepts/runtime_vs_static_typing.md)
- [Variables and Names](/concepts/variables.md)
