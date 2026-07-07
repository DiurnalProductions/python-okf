---
type: Concept
title: Coroutines
description: Native coroutine objects created by async def suspend on await.
tags: [python, async, coroutines, await]
prerequisites:
  - concepts/event_loop
  - concepts/functions_as_objects
related:
  - concepts/asyncio_basics
  - concepts/await_async_lifecycle
  - concepts/generators
resource: "https://docs.python.org/3/glossary.html#term-coroutine"
timestamp: 2026-01-01
---

# Summary

**Coroutines** are functions defined with `async def` that return coroutine objects when called. They can `await` other awaitables, suspending execution without blocking the thread. Coroutines must be driven by an event loop (or awaited from another coroutine).

# Mental model

```
async def fetch():   # defines coroutine function
    ...

coro = fetch()       # coroutine object — body not running yet
await coro           # schedule/continue execution
```

Calling `fetch()` does **not** run the body—like generators, it produces an object to be scheduled. `await` yields control to the loop until the awaited operation completes.

Coroutine functions are distinct from generator functions (`yield`). Old `@asyncio.coroutine` generator-based coroutines are legacy; modern code uses `async def`.

# Example

```python
async def fetch_user(user_id: int) -> dict:
    await asyncio.sleep(0.1)   # simulated I/O
    return {"id": user_id}

async def handler():
    user = await fetch_user(42)
    return user["id"]
```

```python
import inspect
async def job(): pass

inspect.iscoroutinefunction(job)  # True
c = job()
inspect.iscoroutine(c)            # True
```

# Common mistakes

- Forgetting to `await` a coroutine (creates "coroutine was never awaited" warnings).
- Treating `async def` functions like sync functions that return values immediately.
- Confusing coroutines with threads or processes.
- Reusing generator mental model for `send`/`throw` without learning asyncio task APIs.

# Related concepts

- [The Event Loop](/concepts/event_loop.md)
- [asyncio Basics](/concepts/asyncio_basics.md)
- [await and async Lifecycle](/concepts/await_async_lifecycle.md)
