---
id: python.references
type: concept
title: References
description: Assignment and argument passing share object references, not copies.
tags: [python, runtime, references, aliasing]
prerequisites:
  - python.variables
related:
  - python.mutability_immutability
  - python.object_identity_equality
  - python.functions_as_objects
resource: https://docs.python.org/3/faq/programming.html#how-do-i-write-a-function-that-returns-a-reference-to-a-variable
timestamp: 2026-01-01
---

# Summary

Python passes **references to objects** everywhere: assignment, function arguments, and return values all manipulate bindings in namespaces. Multiple names can reference the same object; this aliasing is fundamental to Python's runtime behavior.

# Mental model

Every value in Python is an object in memory. A name is a label pointing at an object. When you pass `my_list` to a function, the parameter name gets bound to the **same list object**—not a copy.

```
a ──→ [1, 2, 3] ←── b
```

Rebinding a parameter inside a function (`param = something_else`) only changes the local name. Mutating the object through any alias (`param.append(4)`) affects all references.

Immutable objects cannot be changed in place, so "modifying" them always creates a new object and rebinds a name.

# Example

```python
def add_item(items, value):
    items.append(value)   # mutates shared list object

data = [1, 2]
add_item(data, 3)
# data is now [1, 2, 3]
```

```python
def rebind(items):
    items = []            # local name only; caller unchanged

rebind(data)
# data unchanged
```

# Common mistakes

- Expecting `def f(x): x = x + 1` to affect the caller's integer (immutables rebind locally).
- Using mutable default arguments (`def f(a=[])`) and sharing one list across calls.
- Confusing rebinding with mutation when debugging shared state bugs.
- Assuming `a = b` copies container contents.

# Related concepts

- [Mutability and Immutability](/concepts/mutability_immutability.md)
- [Object Identity vs Equality](/concepts/object_identity_equality.md)
- [Functions as Objects](/concepts/functions_as_objects.md)
