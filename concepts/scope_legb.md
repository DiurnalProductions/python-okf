---
type: Concept
title: Scope and the LEGB Rule
description: "Name lookup searches Local, Enclosing, Global, then Built-in namespaces."
tags: [python, runtime, scope, legb]
prerequisites:
  - concepts/variables
  - concepts/references
related:
  - concepts/closures
  - concepts/function_execution_lifecycle
  - concepts/imports
resource: "https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces"
timestamp: 2026-01-01
---

# Summary

Python resolves names at runtime using the **LEGB** rule: **L**ocal, **E**nclosing, **G**lobal, **B**uilt-in. Each function call creates a local namespace; modules have global namespaces; nested functions can see enclosing bindings; builtins live in `builtins`.

# Mental model

Scopes are not created by indentation alone—they follow where code is **defined**:

- **Module body** → global namespace (`globals()`)
- **Function body** → local namespace (`locals()` in that frame)
- **Nested def** → inner local + outer enclosing chain
- **Class body** → class namespace (special: does not form an enclosing scope for nested functions' LEGB—methods see class names via different mechanisms)

Assignment binds in the **innermost scope that declares the name local**, unless `global` or `nonlocal` redirects the target. Reading a name walks outward until found or `NameError`.

Indentation tells the parser which block a statement belongs to, which determines which namespace receives bindings.

# Example

```python
x = "global"

def outer():
    x = "enclosing"
    def inner():
        print(x)    # reads enclosing x
    inner()

outer()  # prints "enclosing"
```

```python
count = 0
def increment():
    global count
    count += 1
```

# Common mistakes

- Expecting class scopes to be enclosing scopes for nested functions (they are not for LEGB).
- Using `global` when `nonlocal` is needed in nested functions.
- Creating closures over loop variables without default-arg capture (`lambda i=i: i`).
- Assuming comprehension variables leak in Python 3 (they are local to the comprehension).

# Related concepts

- [Closures](/concepts/closures.md)
- [Function Execution Lifecycle](/concepts/function_execution_lifecycle.md)
- [Imports](/concepts/imports.md)
