---
id: python.function_execution_lifecycle
type: concept
title: Function Execution Lifecycle
description: Calls push frames; returns pop them; objects may outlive their defining frame.
tags: [python, runtime, functions, frames]
prerequisites:
  - python.scope_legb
  - python.interpreter_model
related:
  - python.functions_as_objects
  - python.closures
  - python.generators
resource: https://docs.python.org/3/reference/executionmodel.html#naming-and-binding
timestamp: 2026-01-01
---

# Summary

Calling a function creates a new **stack frame** with its own locals, a reference to globals, and a link to the code object. Returning destroys the frame, but objects created during the call persist if still referenced. Closures and generators extend this model by keeping cells or suspended frames alive.

# Mental model

```
caller frame
  └─ pushes callee frame on call
       locals dict / fast locals
       executes bytecode
  └─ pops frame on return; result object passed to caller
```

Arguments are bound to parameter names (references, not copies). Default values are evaluated **once** at function definition time—critical for mutable defaults.

`return` passes an object reference to the caller. Exceptions unwind the stack until a matching handler is found.

# Example

```python
def greet(name, prefix="Hello"):
    message = f"{prefix}, {name}"
    return message

result = greet("Ada")   # new frame; result binds to returned str object
```

```python
def make():
  defaults = []
  def add(x, target=defaults):
      target.append(x)
      return target
  return add
# default evaluated once at def time — shared list across calls
```

# Common mistakes

- Mutable default arguments sharing state across calls.
- Assuming locals disappear means objects disappear (closures may retain them).
- Confusing `return` with printing (return delivers an object to the caller).
- Ignoring that recursion depth is limited by the C stack / sys recursion limit.

# Related concepts

- [Functions as Objects](/concepts/functions_as_objects.md)
- [Closures](/concepts/closures.md)
- [Generators](/concepts/generators.md)
