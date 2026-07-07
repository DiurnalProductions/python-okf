---
type: Concept
title: Functions as Objects
description: Functions are first-class objects with attributes and callable behavior.
tags: [python, functions, first-class, callable]
prerequisites:
  - concepts/references
  - concepts/function_execution_lifecycle
related:
  - concepts/closures
  - concepts/decorators
  - concepts/classes
resource: "https://docs.python.org/3/tutorial/controlflow.html#defining-functions"
timestamp: 2026-01-01
---

# Summary

A `def` statement creates a **function object** and binds a name to it. Functions can be assigned, passed as arguments, stored in collections, and annotated with attributes. Calling a function invokes its `__call__` protocol via the internal function type.

# Mental model

```
def foo(x): ...
```

At runtime:

1. Build function object with `__code__`, `__defaults__`, `__globals__`, etc.
2. Bind name `foo` to that object
3. `foo(1)` triggers `type(foo).__call__(foo, 1)`

Higher-order functions accept callables—anything with `__call__` (functions, bound methods, classes instances, lambdas).

Lambdas are expression-only function objects with no statements. `def` is preferred for anything non-trivial.

# Example

```python
def apply(fn, value):
    return fn(value)

def double(n):
    return n * 2

apply(double, 21)   # 42
```

```python
def logger(fn):
    def wrapper(*args, **kwargs):
        print(f"calling {fn.__name__}")
        return fn(*args, **kwargs)
    return wrapper
```

# Common mistakes

- Using lambdas where `def` improves readability or debugging (tracebacks show `<lambda>`).
- Forgetting that function attributes like `__defaults__` are shared metadata on the object.
- Assuming `def` and `lambda` create different calling semantics (they do not).
- Storing unbound vs bound methods incorrectly when passing around `obj.method`.

# Related concepts

- [Closures](/concepts/closures.md)
- [Decorators](/concepts/decorators.md)
- [Classes](/concepts/classes.md)
