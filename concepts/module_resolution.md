---
id: python.module_resolution
type: concept
title: Module Resolution
description: Finders and loaders locate modules using sys.path and import hooks.
tags: [python, imports, sys-path, importlib]
prerequisites:
  - python.imports
  - python.modules
related:
  - python.virtual_environments
  - python.pip_packaging
resource: https://docs.python.org/3/reference/import.html#the-import-system
timestamp: 2026-01-01
---

# Summary

Python locates modules by searching **meta path finders** and `sys.path` entries (directories, zip archives, editable installs). Once found, a **loader** executes bytecode and registers the module in `sys.modules`. Namespace packages can span multiple directories without `__init__.py`.

# Mental model

Import sequence (simplified):

1. Check `sys.modules` cache
2. Ask finders on `sys.meta_path` / path hooks
3. Loader creates module object, executes code
4. Store in `sys.modules`

`sys.path` is initialized from:

- Script directory / current working directory semantics
- `PYTHONPATH`
- Standard library
- **site-packages** (third-party installs)

Virtual environments prepend their own `site-packages` while sharing the base interpreter.

# Example

```python
import sys
sys.path.append("/path/to/project")
import mypackage
```

```python
import importlib.util
spec = importlib.util.spec_from_file_location("dyn", "/tmp/mod.py")
mod = importlib.util.module_from_spec(spec)
spec.loader.exec_module(mod)
```

# Common mistakes

- Mutating `sys.path` ad hoc instead of installing packages properly.
- Naming project files `json.py` that shadow the stdlib on path order.
- Assuming `__init__.py` is always required (namespace packages differ).
- Debugging imports without checking `module.__file__` and `sys.path`.

# Related concepts

- [Imports](/concepts/imports.md)
- [Virtual Environments](/concepts/virtual_environments.md)
- [pip and Packaging](/concepts/pip_packaging.md)
