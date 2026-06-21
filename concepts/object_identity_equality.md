---
id: python.object_identity_equality
type: concept
title: Object Identity vs Equality
description: `is` tests object identity; `==` tests value equality via __eq__.
tags: [python, runtime, identity, equality]
prerequisites:
  - python.references
related:
  - python.mutability_immutability
  - python.dunder_methods
  - python.data_model_fundamentals
resource: https://docs.python.org/3/faq/programming.html#how-do-i-check-if-two-objects-are-the-same
timestamp: 2026-01-01
---

# Summary

**Identity** (`is`) asks whether two names reference the **same object** in memory (`id(a) == id(b)`). **Equality** (`==`) asks whether two objects compare equal according to `__eq__`, which may be true for distinct objects with the same value.

# Mental model

Every object has a unique identity for its lifetime (`id(obj)`). Assignment creates aliases with identical identity. Separate constructions may create equal but non-identical objects.

For small integers and some strings, CPython **interns** values, so `is` may appear to work for `==` cases—this is an implementation detail, not a language guarantee.

Equality is customizable per class via `__eq__` and `__hash__`. Mutable objects that override `__eq__` typically set `__hash__ = None` because hashable equality must be stable.

# Example

```python
a = [1, 2, 3]
b = [1, 2, 3]
a == b      # True  — same values
a is b      # False — different list objects

c = a
a is c      # True  — same object
```

```python
x = 256
y = 256
x is y      # may be True or False; do not rely on this

x = 1000
y = 1000
x is y      # typically False on CPython
```

# Common mistakes

- Using `is` to compare strings or numbers for value equality.
- Assuming `==` and `is` are interchangeable for `None`, `True`, `False` (use `is` for singletons).
- Defining `__eq__` without considering hashability for dict keys and set members.
- Expecting `is` to reflect logical sameness across reconstructed objects.

# Related concepts

- [References](/concepts/references.md)
- [Dunder Methods](/concepts/dunder_methods.md)
- [Data Model Fundamentals](/concepts/data_model_fundamentals.md)
