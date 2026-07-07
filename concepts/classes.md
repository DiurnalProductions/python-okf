---
type: Concept
title: Classes
description: Classes are callable objects that construct instances and define shared behavior.
tags: [python, oop, classes, instances]
prerequisites:
  - concepts/functions_as_objects
  - concepts/mutability_immutability
related:
  - concepts/inheritance
  - concepts/dunder_methods
  - concepts/data_model_fundamentals
resource: "https://docs.python.org/3/tutorial/classes.html"
timestamp: 2026-01-01
---

# Summary

A **class** is an object (of `type` or a metaclass) that acts as a factory for **instances**. Instance attributes live in per-object namespaces (`__dict__` or slots); methods are functions retrieved from the class and bound to instances. `class` bodies execute at definition time, creating the class namespace.

# Mental model

```
MyClass (class object)
  ├─ attributes: methods, class variables
  └─ __call__ → creates instance, runs __init__

instance
  ├─ __class__ → MyClass
  └─ instance __dict__ with per-object state
```

`MyClass()` invokes `MyClass.__call__`, which allocates the instance and calls `__init__` if present. Methods are descriptors: `instance.method` binds `self` automatically.

Classes are mutable at runtime—you can add attributes after definition. There is no separate "interface" layer; behavior is whatever attributes exist on the class and its MRO chain.

# Example

```python
class Counter:
    def __init__(self):
        self.value = 0

    def inc(self):
        self.value += 1

c = Counter()
c.inc()
```

```python
class Service:
    timeout = 30   # class variable shared unless shadowed

    def __init__(self, name):
        self.name = name
```

# Common mistakes

- Confusing class variables with instance variables when mutating shared mutable class attrs.
- Omitting `self` in instance method definitions.
- Expecting `__init__` to "return" a new object (it initializes `self`; `__new__` allocates).
- Treating classes as only static schemas—they are live objects participating in the runtime graph.

# Related concepts

- [Inheritance](/concepts/inheritance.md)
- [Dunder Methods](/concepts/dunder_methods.md)
- [Data Model Fundamentals](/concepts/data_model_fundamentals.md)
