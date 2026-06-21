---
id: python.mutability_immutability
type: concept
title: Mutability and Immutability
description: Mutable objects change in place; immutable objects require rebinding.
tags: [python, runtime, mutability, immutability]
prerequisites:
  - python.references
related:
  - python.object_identity_equality
  - python.collections
  - python.classes
resource: https://docs.python.org/3/glossary.html#term-mutable
timestamp: 2026-01-01
---

# Summary

**Mutable** objects can be changed in place after creation (`list`, `dict`, `set`, most user-defined classes). **Immutable** objects cannot (`int`, `float`, `str`, `tuple`, `frozenset`, `bytes`). Whether an object is mutable determines how aliasing and function side effects behave.

# Mental model

Mutation changes the object's internal state while preserving its identity (`id` stays the same). Rebinding a name assigns it to a different object without modifying the original.

```
# mutation
alias.append(4)   # same object, new contents

# rebinding
alias = alias + [4]  # new object; old object unchanged if other refs exist
```

Immutability does not mean "unchangeable variables"—it means the **object** cannot be altered. You can always rebind a name to a new object.

Tuples are immutable containers but may hold references to mutable objects; the tuple cannot be reassigned, but nested mutables can still change.

# Example

```python
row = ([1, 2], "active")
row[0].append(3)    # legal — mutating the inner list
# row is still the same tuple object
```

```python
s = "hello"
s.upper()           # returns new str; s unchanged
s += "!"            # creates new str object, rebinds s
```

# Common mistakes

- Using `+=` on lists thinking it always extends in place (often does for lists, but not for immutable types).
- Treating tuples as deeply immutable when they contain mutable elements.
- Mutating shared mutable defaults or global caches unintentionally.
- Copying a list with `b = a` when `b = a.copy()` or `list(a)` was needed.

# Related concepts

- [References](/concepts/references.md)
- [Collections](/concepts/collections.md)
- [Classes](/concepts/classes.md)
