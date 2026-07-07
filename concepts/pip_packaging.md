---
type: Concept
title: pip and Packaging
description: "pip installs distributions into environments; pyproject.toml defines builds."
tags: [python, pip, packaging, pypi]
prerequisites:
  - concepts/virtual_environments
  - concepts/modules
related:
  - concepts/module_resolution
resource: "https://docs.python.org/3/installing/index.html"
timestamp: 2026-01-01
---

# Summary

**pip** downloads and installs **distribution packages** (wheels, sdists) from PyPI or other indexes into `site-packages`. Modern projects declare metadata and build backends in **`pyproject.toml`** (PEP 621, PEP 517). Installed packages become importable modules according to module resolution rules.

# Mental model

```
pyproject.toml  →  build backend produces wheel
pip install .   →  extract into site-packages/
import pkg      →  import system loads pkg namespace
```

Key concepts:

- **Distribution** — what pip installs (may contain multiple import packages)
- **Import package** — what `import` loads (`mypkg/`)
- **Dependencies** — resolved recursively into the environment
- **Extras** — optional dependency sets (`pip install pkg[dev]`)

Lock files (`pip-tools`, Poetry, uv) pin versions for reproducible environments.

# Example

```toml
# pyproject.toml
[project]
name = "demo"
version = "0.1.0"
dependencies = ["httpx>=0.27"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```bash
pip install -e ".[dev]"
```

# Common mistakes

- Installing into system Python on macOS/Linux and breaking OS tooling.
- Confusing `pip install package` with importing a similarly named module you wrote locally.
- Omitting upper bounds or lockfiles in applications that need reproducibility.
- Using `requirements.txt` from `pip freeze` with unintended transitive pins without curation.

# Related concepts

- [Virtual Environments](/concepts/virtual_environments.md)
- [Module Resolution](/concepts/module_resolution.md)
- [Modules](/concepts/modules.md)
