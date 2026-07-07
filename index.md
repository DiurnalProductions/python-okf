---
okf_version: "0.1"
id: python-okf
name: Python Knowledge Pack
version: "0.1"
description: "OKF knowledge base for Python 3 (runtime model, data model, async, typing, modules, ecosystem)"
tags: [python, programming, async, oop, data-model, scripting]
---

# Python Knowledge Pack

A graph-based OKF knowledge bundle for modern Python (3.10+). This pack treats Python as a **dynamic object reference system**: names bind to objects at runtime, behavior emerges from the object model, and syntax is the surface over a strongly structured interpreter.

## Start here

If you are new to Python's runtime semantics (not just its syntax), begin with these concepts in order:

1. [Variables and Names](/concepts/variables.md) — names are bindings, not boxes
2. [References](/concepts/references.md) — assignment shares objects; aliasing is real
3. [Mutability and Immutability](/concepts/mutability_immutability.md) — why bugs happen through shared state
4. [The Interpreter Model](/concepts/interpreter_model.md) — how code becomes runtime objects

From there, follow the learning paths below based on your goal.

## Python as a dynamic object reference system

At runtime, Python programs are not "variables holding values." They are:

- **Objects** living in memory (integers, lists, functions, classes, modules)
- **Names** in namespaces that reference those objects
- **Bytecode** executed by a virtual machine that manipulates the object graph
- **Protocols** (iteration, context management, async) implemented via dunder methods

Indentation defines block structure because the parser uses it to build the same scope tree the runtime uses. Type hints annotate source code but do not change runtime behavior unless you add explicit tooling.

## Recommended learning progression

### Runtime model

* [Variables and Names](/concepts/variables.md) — binding names to objects
* [References](/concepts/references.md) — shared object identity through assignment
* [Dynamic Typing](/concepts/dynamic_typing.md) — types belong to objects, not names
* [Object Identity vs Equality](/concepts/object_identity_equality.md) — `is` vs `==`
* [Mutability and Immutability](/concepts/mutability_immutability.md) — in-place change vs rebinding
* [The Interpreter Model](/concepts/interpreter_model.md) — CPython execution overview
* [Bytecode](/concepts/bytecode.md) — compiled instructions and the eval loop
* [Scope and the LEGB Rule](/concepts/scope_legb.md) — where names are resolved
* [Function Execution Lifecycle](/concepts/function_execution_lifecycle.md) — frames, calls, and returns

### Functions

* [Functions as Objects](/concepts/functions_as_objects.md) — first-class callables
* [Closures](/concepts/closures.md) — functions capturing enclosing bindings
* [Decorators](/concepts/decorators.md) — syntactic sugar over higher-order functions

### Object system

* [Classes](/concepts/classes.md) — types as callable factories
* [Inheritance](/concepts/inheritance.md) — sharing and extending behavior
* [Method Resolution Order (MRO)](/concepts/mro.md) — deterministic lookup algorithm
* [Dunder Methods](/concepts/dunder_methods.md) — hooks into Python protocols
* [Data Model Fundamentals](/concepts/data_model_fundamentals.md) — the object protocol as a contract

### Data structures

* [Collections](/concepts/collections.md) — lists, dicts, sets, tuples at runtime
* [The Iteration Protocol](/concepts/iteration_protocol.md) — `__iter__` and `__next__`
* [Generators](/concepts/generators.md) — lazy iteration via suspended frames

### Async system

* [The Event Loop](/concepts/event_loop.md) — cooperative scheduling model
* [Coroutines](/concepts/coroutines.md) — suspendable functions as objects
* [asyncio Basics](/concepts/asyncio_basics.md) — tasks, futures, and the stdlib loop
* [await and async Lifecycle](/concepts/await_async_lifecycle.md) — how suspension and resumption work

### Typing system

* [Type Hints](/concepts/type_hints.md) — annotations without runtime enforcement
* [The typing Module](/concepts/typing_module.md) — composable static types
* [Optional, Union, and Generics](/concepts/optional_union_generics.md) — modern type composition
* [Runtime vs Static Typing](/concepts/runtime_vs_static_typing.md) — what checkers see vs what runs

### Ecosystem and modules

* [Modules](/concepts/modules.md) — namespaces as importable objects
* [Imports](/concepts/imports.md) — binding module objects into scopes
* [Module Resolution](/concepts/module_resolution.md) — `sys.path` and import machinery
* [Virtual Environments](/concepts/virtual_environments.md) — isolated dependency islands
* [pip and Packaging](/concepts/pip_packaging.md) — installing and distributing packages

## Concept graph

Prerequisites form a directed acyclic graph. Each concept file lists `prerequisites` in frontmatter; follow those edges for dependency-safe reading.

Primary flows:

- **Core model:** variables → references → mutability → functions → closures
- **Object system:** classes → inheritance → MRO → dunder methods
- **Async:** event loop → coroutines → asyncio → await lifecycle
- **Ecosystem:** modules → imports → virtual environments → packaging
