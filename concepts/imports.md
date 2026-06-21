---
id: python.imports
type: concept
title: Imports
description: Import statements bind names to module objects or their attributes.
tags: [python, imports, binding, namespace]
prerequisites:
  - python.modules
  - python.scope_legb
related:
  - python.module_resolution
resource: https://docs.python.org/3/reference/import.html
timestamp: 2026-01-01
---

# Summary

`import` and `from ... import` are binding operations executed at runtime. They locate modules, ensure initialization, and assign names in the current scope. Import machinery caches modules in `sys.modules` so each module initializes once per interpreter (unless reloaded).

# Mental model

Forms:

- `import mod` → bind `mod` to module object
- `import mod as alias` → bind `alias` to same module object
- `from mod import name` → bind `name` to `mod.name` (reference to attribute object)
- `from mod import name as alias` → bind `alias` to `mod.name`

`from pkg import item` may import a submodule or an attribute depending on `pkg.__path__` and loaders.

Relative imports (`from . import sibling`) use package context (`__package__`, `__spec__`).

# Example

```python
import json
from pathlib import Path

data = json.loads('{"a": 1}')
p = Path(".")
```

```python
# package/submodule.py accessed as:
from mypkg import submodule
# or
from mypkg.submodule import helper
```

# Common mistakes

- Expecting `from mod import obj` to copy immutable snapshots when `mod.obj` later changes (names rebind on reimport; attribute access is live).
- Shadowing standard library names (`import json` then `json = ...`).
- Using relative imports in scripts run as `__main__` without package context.
- Import-time circular dependencies between modules.

# Related concepts

- [Modules](/concepts/modules.md)
- [Module Resolution](/concepts/module_resolution.md)
- [Scope and the LEGB Rule](/concepts/scope_legb.md)
