---
id: python.collections
type: concept
title: Collections
description: Lists, dicts, sets, and tuples are optimized mutable and immutable containers.
tags: [python, data-structures, list, dict, set, tuple]
prerequisites:
  - python.mutability_immutability
  - python.references
related:
  - python.iteration_protocol
  - python.data_model_fundamentals
resource: https://docs.python.org/3/tutorial/datastructures.html
timestamp: 2026-01-01
---

# Summary

Python's core **collections** are objects with well-defined performance characteristics and protocol support. **Lists** and **dicts** are ordered (lists) or insertion-ordered (dicts, 3.7+ guaranteed) mutable mappings/sequences. **Sets** are mutable unordered unique collections. **Tuples** are immutable sequences, often used for fixed records and dict keys.

# Mental model

All are objects referencing other objects:

```
list  → dynamic array of pointers
dict  → hash table mapping hashable keys to values
set   → hash table of keys only
tuple → fixed-size array of pointers (immutable)
```

Operations like `append`, `update`, and slice assignment mutate lists in place. Dict/set membership uses `__hash__` and `__eq__` on keys. Views (`dict.keys()`, etc.) are dynamic proxy objects.

Literals (`[]`, `{}`, `set()`, `()`) construct objects; comprehensions build them via dedicated bytecode loops.

# Example

```python
users = [
    {"id": 1, "name": "Ada"},
    {"id": 2, "name": "Lin"},
]

index = {u["id"]: u for u in users}
seen = {1, 2, 3}
point = (10, 20)
```

```python
row = ([1, 2], "ok")
row[0].append(3)   # tuple fixed; inner list mutable
```

# Common mistakes

- Using lists as dict keys (unhashable).
- Relying on set ordering before assuming insertion order (sets are unordered).
- `{}` creating empty dict, not set—use `set()` for empty sets.
- Shallow copies when deep copies are needed for nested structures.
- Confusing `tuple` immutability with deep immutability of elements.

# Related concepts

- [Mutability and Immutability](/concepts/mutability_immutability.md)
- [The Iteration Protocol](/concepts/iteration_protocol.md)
- [Data Model Fundamentals](/concepts/data_model_fundamentals.md)
