---
type: Concept
title: Data Model Fundamentals
description: "Python's object protocol unifies builtins, operators, and user-defined types."
tags: [python, oop, data-model, protocol]
prerequisites:
  - concepts/dunder_methods
  - concepts/object_identity_equality
related:
  - concepts/iteration_protocol
  - concepts/collections
resource: "https://docs.python.org/3/reference/datamodel.html"
timestamp: 2026-01-01
---

# Summary

The **data model** is the contract that all Python objects follow: identity, type, reference count, attribute access, and protocol hooks. Built-in types and user classes participate in the same system—`for`, `with`, `await`, and arithmetic all dispatch through dunder methods and descriptors.

# Mental model

Everything is an object with:

- **Identity** — stable `id` for lifetime
- **Type** — `__class__` determines behavior
- **Namespace** — `__dict__` or `__slots__` for attributes
- **Protocol participation** — optional dunder implementations

Attribute access `obj.name` runs `type(obj).__getattribute__(obj, name)` which respects descriptors on the class MRO. This unifies functions-as-methods, `@property`, and class/static methods.

Understanding the data model shifts focus from syntax to **what operation the interpreter requests from objects**.

# Example

```python
class Stack:
    def __init__(self):
        self._items = []

    def __len__(self):
        return len(self._items)

    def __getitem__(self, index):
        return self._items[index]

    def push(self, item):
        self._items.append(item)
```

```python
# for item in stack:  → iter(stack) → stack.__iter__
class Stack:
    def __iter__(self):
        return iter(self._items)
```

# Common mistakes

- Fighting the data model with heavy `__getattr__` when explicit methods are clearer.
- Subclassing built-ins (`class MyList(list)`) without understanding method overrides and `super()` on builtins.
- Implementing partial protocols (e.g., `__getitem__` without considering `__len__` for slicing expectations).
- Assuming special methods are called on the right operand only in rich comparisons.

# Related concepts

- [Dunder Methods](/concepts/dunder_methods.md)
- [The Iteration Protocol](/concepts/iteration_protocol.md)
- [Collections](/concepts/collections.md)
