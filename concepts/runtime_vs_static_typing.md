---
id: python.runtime_vs_static_typing
type: concept
title: Runtime vs Static Typing
description: Python enforces types at operation time; checkers analyze annotations separately.
tags: [python, typing, runtime, static-analysis]
prerequisites:
  - python.type_hints
  - python.dynamic_typing
related:
  - python.typing_module
resource: https://docs.python.org/3/library/typing.html#module-contents
timestamp: 2026-01-01
---

# Summary

**Runtime typing** is dynamic: operations consult actual object types when executed. **Static typing** is optional analysis of annotations before or alongside development. The two layers are deliberately separate—annotations are not Python's primary enforcement mechanism.

# Mental model

| Concern | Runtime | Static (mypy/pyright) |
|---------|---------|------------------------|
| `x: int = "a"` | Legal binding | Type error reported |
| `3 + "x"` | `TypeError` at line | Error predicted |
| Duck typing | Works if methods exist | May warn without Protocol |
| Refactors | Tests catch issues | Checker catches API drift |

Runtime helpers like `isinstance` and `typing.cast` serve different purposes: `isinstance` is real checks; `cast` tells checkers only, no runtime effect.

Libraries (`pydantic`, `typeguard`) can bridge layers with explicit validation.

# Example

```python
def add(a: int, b: int) -> int:
    return a + b

add("1", "2")   # runtime: TypeError on + 
                # static checker: error before run
```

```python
from typing import cast

value: object = get_data()
name = cast(str, value)   # checker trusts str; runtime unchanged
```

# Common mistakes

- Assuming mypy-clean code cannot fail at runtime.
- Using `cast` to silence errors instead of fixing logic.
- Enabling strict checkers without CI integration, letting drift accumulate.
- Expecting JSON/API data to match hints without validation boundaries.

# Related concepts

- [Type Hints](/concepts/type_hints.md)
- [Dynamic Typing](/concepts/dynamic_typing.md)
- [The typing Module](/concepts/typing_module.md)
