---
id: python.asyncio_basics
type: concept
title: asyncio Basics
description: Stdlib framework for event loops, tasks, futures, and async I/O.
tags: [python, async, asyncio, tasks]
prerequisites:
  - python.coroutines
  - python.event_loop
related:
  - python.await_async_lifecycle
resource: https://docs.python.org/3/library/asyncio.html
timestamp: 2026-01-01
---

# Summary

**asyncio** is Python's standard library for writing concurrent I/O-bound programs using coroutines. It provides the event loop, `Task` wrappers for concurrent coroutines, `Future` for deferred results, synchronization primitives, and async streams.

# Mental model

Key objects:

- **Event loop** — scheduler (`asyncio.get_running_loop()`)
- **Coroutine** — `async def` product awaiting scheduling
- **Task** — `asyncio.create_task(coro)` schedules concurrent execution on the loop
- **Future** — low-level placeholder for a result set by callbacks

`asyncio.run(main())` creates a loop, runs `main` to completion, and cleans up—preferred entry for scripts.

`asyncio.gather` and `TaskGroup` (3.11+) coordinate multiple awaitables with structured concurrency patterns.

# Example

```python
import asyncio

async def fetch(n):
    await asyncio.sleep(0.1)
    return n * 10

async def main():
    tasks = [asyncio.create_task(fetch(i)) for i in range(3)]
    results = await asyncio.gather(*tasks)
    return results

asyncio.run(main())   # [0, 10, 20]
```

```python
async def main():
    async with asyncio.TaskGroup() as tg:
        t1 = tg.create_task(fetch(1))
        t2 = tg.create_task(fetch(2))
    return t1.result(), t2.result()
```

# Common mistakes

- Creating tasks without retaining references (GC may cancel them in edge cases).
- Using `gather` without `return_exceptions=True` when partial failure is acceptable.
- Sharing mutable state across tasks without locks.
- Assuming `create_task` runs immediately before next await (scheduling is cooperative).

# Related concepts

- [Coroutines](/concepts/coroutines.md)
- [The Event Loop](/concepts/event_loop.md)
- [await and async Lifecycle](/concepts/await_async_lifecycle.md)
