---
id: python.decorators
type: concept
title: Decorators
description: @syntax applies a callable to a function or class at definition time.
tags: [python, functions, decorators, syntax]
prerequisites:
  - python.functions_as_objects
  - python.closures
related:
  - python.classes
  - python.type_hints
resource: https://docs.python.org/3/glossary.html#term-decorator
timestamp: 2026-01-01
---

# Summary

Decorators are **callable wrappers** applied when a function or class is defined. `@decorator` above `def f` is equivalent to `f = decorator(f)`. Decorators are higher-order functions (or callable objects) that return a replacement callable, often preserving metadata with `functools.wraps`.

# Mental model

Definition time, not call time:

```python
@logged
def work(): ...
# binds name work to logged(work) immediately
```

A decorator receives the original function object and returns a new callable—usually a closure—that may run code before/after delegating to the original. Class decorators receive the class object and return a (possibly modified) class.

Stacking decorators applies bottom-up: `@a @b def f` → `f = a(b(f))`.

# Example

```python
from functools import wraps

def retry(times):
    def decorator(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            for attempt in range(times):
                try:
                    return fn(*args, **kwargs)
                except Exception:
                    if attempt == times - 1:
                        raise
        return wrapper
    return decorator

@retry(3)
def fetch():
    ...
```

```python
@dataclass
class Point:
    x: int
    y: int
```

# Common mistakes

- Forgetting `@wraps`, losing `__name__` and `__doc__` on wrapped functions.
- Writing decorators that forget to return a callable.
- Expecting decorators to run on each call (they run once at definition unless wrapper does per-call work).
- Confusing parametrized decorators (`@deco(arg)`) with bare decorators—extra call returns the actual decorator.

# Related concepts

- [Closures](/concepts/closures.md)
- [Functions as Objects](/concepts/functions_as_objects.md)
- [Classes](/concepts/classes.md)
