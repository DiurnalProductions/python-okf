---
id: python.virtual_environments
type: concept
title: Virtual Environments
description: Isolated Python environments with separate site-packages and executables.
tags: [python, venv, environment, isolation]
prerequisites:
  - python.modules
related:
  - python.pip_packaging
  - python.module_resolution
resource: https://docs.python.org/3/library/venv.html
timestamp: 2026-01-01
---

# Summary

A **virtual environment** is a lightweight prefix layout: an interpreter shim, `pyvenv.cfg`, and a private `site-packages` directory. It isolates installed third-party packages per project while reusing the base Python standard library and binary.

# Mental model

```
project/.venv/
  bin/python      → shim to base interpreter
  pyvenv.cfg      → home = base Python path
  lib/python3.x/site-packages/  → project deps
```

Activating (`source .venv/bin/activate`) adjusts `PATH` so `python` and `pip` target the venv. `sys.prefix` and `sys.base_prefix` differ when running inside a venv.

Environments are not full VM isolation—same OS user, same system libraries—but dependency graphs are separated.

# Example

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install requests
python -c "import requests; print(requests.__file__)"
```

```python
import sys
sys.prefix != sys.base_prefix   # True inside venv
```

# Common mistakes

- Committing `.venv` to version control instead of `requirements.txt` / `pyproject.toml`.
- Installing packages globally when a venv exists.
- Mixing conda and venv mental models (different tooling).
- Forgetting to recreate venv after major Python upgrades.

# Related concepts

- [pip and Packaging](/concepts/pip_packaging.md)
- [Module Resolution](/concepts/module_resolution.md)
- [Modules](/concepts/modules.md)
