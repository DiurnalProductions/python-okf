---
type: Concept
title: The Interpreter Model
description: CPython compiles source to bytecode and executes it on a stack-based VM.
tags: [python, runtime, interpreter, cpython]
related:
  - concepts/bytecode
  - concepts/function_execution_lifecycle
  - concepts/modules
  - concepts/event_loop
resource: "https://docs.python.org/3/reference/executionmodel.html"
timestamp: 2026-01-01
---

# Summary

CPython—the reference implementation—parses source into an abstract syntax tree, compiles it to **bytecode**, and executes that bytecode on an evaluation loop. At runtime, everything you manipulate is an object: integers, functions, modules, and even bytecode itself.

# Mental model

Execution layers:

1. **Source** → tokenizer/parser → AST
2. **AST** → compiler → code object (bytecode + constants + names)
3. **Interpreter** → fetches opcodes, operates on a value stack and frame locals

The global interpreter lock (GIL) in CPython means one thread executes Python bytecode at a time per process, which affects CPU-bound threading but not I/O-bound async patterns.

Objects are heap-allocated C structures with reference counts (plus cyclic GC). Names in scopes are mappings to object pointers.

# Example

```python
import dis

def add(a, b):
    return a + b

dis.dis(add)   # view bytecode opcodes LOAD_FAST, BINARY_OP, RETURN_VALUE
```

```python
# Everything is an object
print(type(add))           # <class 'function'>
print(add.__code__)        # code object with bytecode
```

# Common mistakes

- Assuming Python is "interpreted line by line" without compilation to bytecode.
- Expecting true CPU parallelism from threads in CPython for pure Python compute.
- Confusing the language specification with CPython implementation details (though they often align).
- Ignoring that import executes module top-level code once per process (usually).

# Related concepts

- [Bytecode](/concepts/bytecode.md)
- [Function Execution Lifecycle](/concepts/function_execution_lifecycle.md)
- [Modules](/concepts/modules.md)
