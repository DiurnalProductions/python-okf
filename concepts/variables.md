---
type: Concept
title: Variables and Names
description: "Names in Python are bindings to objects, not typed storage slots."
tags: [python, runtime, names, binding]
related:
  - concepts/references
  - concepts/dynamic_typing
  - concepts/scope_legb
resource: "https://docs.python.org/3/tutorial/introduction.html"
timestamp: 2026-01-01
---

# Summary

In Python, a "variable" is really a **name** in a namespace that **references** an object. Assignment does not copy data into a named slot; it binds a name to an object that already exists or is newly created. Names have no intrinsic type—only the objects they reference do.

# Mental model

Think of Python namespaces as dictionaries mapping strings to object references:

```
namespaces["x"] → <int object 42 at 0x...>
```

When you write `x = 42`, the interpreter:

1. Creates or finds the integer object `42`
2. Binds the name `x` in the current scope to that object

Rebinding `x` to another value does not mutate the old object—it points `x` at a different object. Indentation defines which namespace receives the binding because block structure determines scope boundaries at parse time.

There are no declarations. A name exists because it was assigned, imported, or passed as a parameter. Reading an unbound name raises `NameError`.

# Example

```python
x = 10          # bind name x to int object 10
y = x           # bind y to the same object as x
x = 20          # rebind x; y still references 10
```

```python
def f():
    a = 1       # local binding in f's namespace

f()             # a is not visible here
```

# Common mistakes

- Treating names as typed containers (like `int x` in C). Types live on objects.
- Assuming assignment always copies. It only rebinds names unless you mutate a mutable object in place.
- Using `=` when you mean "update contents" on a mutable object shared by multiple names.
- Expecting block-scoped variables like JavaScript `let`; Python has function/module/class scope rules (see LEGB).

# Related concepts

- [References](/concepts/references.md) — how multiple names share one object
- [Dynamic Typing](/concepts/dynamic_typing.md) — why names are untyped
- [Scope and the LEGB Rule](/concepts/scope_legb.md) — where bindings live
