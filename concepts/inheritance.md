---
type: Concept
title: Inheritance
description: "Subclasses extend superclasses; attribute lookup walks the MRO chain."
tags: [python, oop, inheritance, subclass]
prerequisites:
  - concepts/classes
related:
  - concepts/mro
  - concepts/dunder_methods
resource: "https://docs.python.org/3/tutorial/classes.html#inheritance"
timestamp: 2026-01-01
---

# Summary

**Inheritance** lets a subclass reuse and override attributes from one or more base classes. Python supports single inheritance syntax with multiple bases in the class header. Method lookup is not "parent first intuition"—it follows the **method resolution order (MRO)** computed by the C3 linearization algorithm.

# Mental model

```
class Child(Parent):
    ...
```

Creates class object `Child` with `__bases__` and `__mro__` tuple. Attribute access on instance or class uses descriptor protocol and MRO search—not merely the first matching parent.

`super()` returns a proxy that delegates to the next class in the MRO from the calling context, enabling cooperative multiple inheritance.

Instance `__dict__` holds instance data; class hierarchies share method objects unless overridden.

# Example

```python
class Animal:
    def speak(self):
        return "..."

class Dog(Animal):
    def speak(self):
        return "woof"

Dog().speak()   # "woof" — override on subclass
```

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return super().method() + "+B"
```

# Common mistakes

- Calling parent methods with hard-coded `Parent.method(self)` instead of `super()` in diamond hierarchies.
- Assuming inheritance order in the class header equals lookup order (MRO decides).
- Using inheritance for code reuse only when composition is clearer.
- Overriding `__init__` without chaining to cooperative `super().__init__` calls.

# Related concepts

- [Method Resolution Order (MRO)](/concepts/mro.md)
- [Classes](/concepts/classes.md)
- [Dunder Methods](/concepts/dunder_methods.md)
