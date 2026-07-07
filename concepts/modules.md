---
type: Concept
title: Modules
description: Modules are namespace objects loaded from .py files or extension modules.
tags: [python, modules, namespace, import-system]
prerequisites:
  - concepts/interpreter_model
related:
  - concepts/imports
  - concepts/module_resolution
  - concepts/virtual_environments
resource: "https://docs.python.org/3/tutorial/modules.html"
timestamp: 2026-01-01
---

# Summary

A **module** is an object (instance of `types.ModuleType`) whose attributes form a namespace for definitions. Executing a module's top-level code populates that namespace once; subsequent imports retrieve the cached module from `sys.modules`.

# Mental model

```
mymodule.py  →  module object in sys.modules["mymodule"]
                  ├─ x = 1
                  ├─ def f(): ...
                  └─ __name__, __file__, __package__
```

Packages are modules with `__path__` (directories with `__init__.py` or namespace packages). The module object is the unit of reuse—import binds names to it or its attributes.

Module-level code runs at import time—side effects (I/O, global setup) happen then.

# Example

```python
# config.py
DEBUG = True

def settings():
    return {"debug": DEBUG}
```

```python
import config
config.DEBUG = False
```

# Common mistakes

- Putting executable side effects in modules imported widely.
- Circular imports causing partially initialized modules.
- Confusing module namespace with class or function local namespaces.
- Relying on `from module import *` in production code (pollutes namespace).

# Related concepts

- [Imports](/concepts/imports.md)
- [Module Resolution](/concepts/module_resolution.md)
- [Virtual Environments](/concepts/virtual_environments.md)
