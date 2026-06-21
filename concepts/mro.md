---
id: python.mro
type: concept
title: Method Resolution Order (MRO)
description: C3 linearization determines the deterministic order for attribute lookup.
tags: [python, oop, mro, inheritance]
prerequisites:
  - python.inheritance
related:
  - python.dunder_methods
  - python.classes
resource: https://docs.python.org/3/glossary.html#term-method-resolution-order
timestamp: 2026-01-01
---

# Summary

The **MRO** is a tuple on each class (`Class.__mro__`) listing the class and its bases in the order Python searches for attributes. C3 linearization guarantees a consistent order that respects local precedence and monotonicity in multiple inheritance diamonds.

# Mental model

On `instance.attr`:

1. Check instance `__dict__` (and slots if defined)
2. Walk `type(instance).__mro__` for data descriptors on classes
3. Fall back to instance `__dict__` for non-descriptors
4. Walk MRO again for non-data descriptors and plain attributes
5. Raise `AttributeError` if not found

`super()` uses the MRO of the defining class and the current `__class__` closure cell—not "the parent class" in the English sense.

Inspect with `Class.mro()` or `help(Class)` to debug unexpected overrides in complex hierarchies.

# Example

```python
class Base: ...
class Left(Base): ...
class Right(Base): ...
class Child(Left, Right): ...

Child.__mro__
# (Child, Left, Right, Base, object)
```

```python
class LoggingMixin:
    def save(self):
        print("logging")
        return super().save()

class Model:
    def save(self):
        print("persist")
```

# Common mistakes

- Designing diamond hierarchies without understanding `super()` cooperation.
- Assuming the leftmost base in `class C(A, B)` always "wins" for every attribute without checking MRO.
- Creating inconsistent MRO by mixing classic and new-style patterns (all classes inherit `object` in Python 3).
- Debugging overrides without printing `__mro__`.

# Related concepts

- [Inheritance](/concepts/inheritance.md)
- [Dunder Methods](/concepts/dunder_methods.md)
- [Classes](/concepts/classes.md)
