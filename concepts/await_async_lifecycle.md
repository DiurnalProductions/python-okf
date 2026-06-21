---
id: python.await_async_lifecycle
type: concept
title: await and async Lifecycle
description: How await suspends coroutines and resumes them when awaitables complete.
tags: [python, async, await, lifecycle]
prerequisites:
  - python.asyncio_basics
  - python.coroutines
related:
  - python.event_loop
resource: https://docs.python.org/3/reference/expressions.html#await-expression
timestamp: 2026-01-01
---

# Summary

The `await` expression can only appear inside `async def`. It **suspends** the current coroutine until the awaited **awaitable** completes, registering a callback with the event loop. The coroutine frame is preserved; locals and execution point survive across suspensions.

# Mental model

Lifecycle of `result = await thing`:

1. Evaluate `thing` — must be awaitable (coroutine, Task, Future, or object with `__await__`)
2. If not done, suspend current coroutine frame
3. Loop runs other ready work
4. On completion, resume after `await` with result (or raise exception into frame)

`async with` and `async for` desugar to `__aenter__`/`__aexit__` and async iterator protocols using the same suspension machinery.

Returning from `async def` wraps the value in a coroutine that completes with that result when awaited.

# Example

```python
async def step1():
    await asyncio.sleep(0)
    return 1

async def pipeline():
    a = await step1()
    b = await step1()
    return a + b
```

```python
class AsyncReader:
    def __init__(self, items):
        self.items = items

    def __aiter__(self):
        self._it = iter(self.items)
        return self

    async def __anext__(self):
        try:
            return next(self._it)
        except StopIteration:
            raise StopAsyncIteration
```

# Common mistakes

- Using `await` outside `async def` (syntax error).
- Awaiting a coroutine function instead of calling it first (`await fetch` vs `await fetch()`).
- Blocking the loop during await chains by hidden sync code in libraries.
- Ignoring cancellation (`asyncio.CancelledError`) in cleanup paths.

# Related concepts

- [Coroutines](/concepts/coroutines.md)
- [asyncio Basics](/concepts/asyncio_basics.md)
- [The Event Loop](/concepts/event_loop.md)
