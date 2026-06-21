---
id: python.bytecode
type: concept
title: Bytecode
description: Compiled stack instructions that the CPython eval loop executes.
tags: [python, runtime, bytecode, compiler]
prerequisites:
  - python.interpreter_model
related:
  - python.function_execution_lifecycle
  - python.scope_legb
resource: https://docs.python.org/3/library/dis.html
timestamp: 2026-01-01
---

# Summary

Bytecode is the intermediate representation CPython executes. Each function, module, and comprehension body has an associated **code object** containing opcodes, a constant pool, and name tuples (`co_varnames`, `co_names`). Understanding bytecode at a high level clarifies how scopes and closures are implemented.

# Mental model

The eval loop is a `while True: opcode = next(); dispatch(opcode)` machine operating on:

- **Value stack** — temporary operands
- **Frame** — links to locals, globals, builtins, and the code object
- **Block stack** — tracks loops, except handlers, and context managers

Opcodes like `LOAD_FAST`, `STORE_NAME`, and `MAKE_CELL` map directly to name binding and closure cell creation. You rarely write bytecode, but it explains why local rebinding differs from `global`/`nonlocal` declarations.

# Example

```python
import dis

code = compile("x = x + 1", "<string>", "exec")
dis.dis(code)
# LOAD_NAME, LOAD_CONST, BINARY_OP, STORE_NAME pattern
```

```python
def outer():
    x = 1
    def inner():
        nonlocal x
        x += 1
    return inner
# MAKE_CELL / LOAD_DEREF opcodes implement closure cells
```

# Common mistakes

- Micro-optimizing based on opcode folklore without profiling.
- Assuming bytecode is portable across Python versions (opcodes evolve).
- Treating `.pyc` files as a separate language—they cache bytecode for faster startup.
- Believing `eval`/`exec` bypass the object model (they compile and run in a namespace).

# Related concepts

- [The Interpreter Model](/concepts/interpreter_model.md)
- [Function Execution Lifecycle](/concepts/function_execution_lifecycle.md)
- [Scope and the LEGB Rule](/concepts/scope_legb.md)
