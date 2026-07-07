# Concepts

* [asyncio Basics](asyncio_basics.md) - Stdlib framework for event loops, tasks, futures, and async I/O.
* [await and async Lifecycle](await_async_lifecycle.md) - How await suspends coroutines and resumes them when awaitables complete.
* [Bytecode](bytecode.md) - Compiled stack instructions that the CPython eval loop executes.
* [Classes](classes.md) - Classes are callable objects that construct instances and define shared behavior.
* [Closures](closures.md) - Inner functions that retain references to enclosing bindings via cells.
* [Collections](collections.md) - Lists, dicts, sets, and tuples are optimized mutable and immutable containers.
* [Coroutines](coroutines.md) - Native coroutine objects created by async def suspend on await.
* [Data Model Fundamentals](data_model_fundamentals.md) - Python's object protocol unifies builtins, operators, and user-defined types.
* [Decorators](decorators.md) - @syntax applies a callable to a function or class at definition time.
* [Dunder Methods](dunder_methods.md) - Double-underscore hooks customize how objects participate in Python protocols.
* [Dynamic Typing](dynamic_typing.md) - Object types are determined at runtime; names carry no static type.
* [The Event Loop](event_loop.md) - Cooperative scheduler that runs coroutines until they await I/O or timers.
* [Function Execution Lifecycle](function_execution_lifecycle.md) - Calls push frames; returns pop them; objects may outlive their defining frame.
* [Functions as Objects](functions_as_objects.md) - Functions are first-class objects with attributes and callable behavior.
* [Generators](generators.md) - Generator functions suspend execution, yielding values via frozen frames.
* [Imports](imports.md) - Import statements bind names to module objects or their attributes.
* [Inheritance](inheritance.md) - Subclasses extend superclasses; attribute lookup walks the MRO chain.
* [The Interpreter Model](interpreter_model.md) - CPython compiles source to bytecode and executes it on a stack-based VM.
* [The Iteration Protocol](iteration_protocol.md) - Iterables expose __iter__; iterators expose __next__ until StopIteration.
* [Module Resolution](module_resolution.md) - Finders and loaders locate modules using sys.path and import hooks.
* [Modules](modules.md) - Modules are namespace objects loaded from .py files or extension modules.
* [Method Resolution Order (MRO)](mro.md) - C3 linearization determines the deterministic order for attribute lookup.
* [Mutability and Immutability](mutability_immutability.md) - Mutable objects change in place; immutable objects require rebinding.
* [Object Identity vs Equality](object_identity_equality.md) - `is` tests object identity; `==` tests value equality via __eq__.
* [Optional, Union, and Generics](optional_union_generics.md) - Express nullable, alternates, and parameterized types for checkers.
* [pip and Packaging](pip_packaging.md) - pip installs distributions into environments; pyproject.toml defines builds.
* [References](references.md) - Assignment and argument passing share object references, not copies.
* [Runtime vs Static Typing](runtime_vs_static_typing.md) - Python enforces types at operation time; checkers analyze annotations separately.
* [Scope and the LEGB Rule](scope_legb.md) - Name lookup searches Local, Enclosing, Global, then Built-in namespaces.
* [Type Hints](type_hints.md) - Annotations document expected types without changing runtime behavior.
* [The typing Module](typing_module.md) - Composable type constructors for static analysis and documentation.
* [Variables and Names](variables.md) - Names in Python are bindings to objects, not typed storage slots.
* [Virtual Environments](virtual_environments.md) - Isolated Python environments with separate site-packages and executables.
