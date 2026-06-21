---
id: python.event_loop
type: concept
title: The Event Loop
description: Cooperative scheduler that runs coroutines until they await I/O or timers.
tags: [python, async, event-loop, concurrency]
prerequisites:
  - python.interpreter_model
related:
  - python.coroutines
  - python.asyncio_basics
resource: https://docs.python.org/3/library/asyncio-eventloop.html
timestamp: 2026-01-01
---

# Summary

An **event loop** is the runtime scheduler for async code. It maintains a queue of ready coroutines and I/O callbacks, executing one coroutine until it **awaits** something, then switching to other work. This is **cooperative multitasking**, not parallel threads.

# Mental model

```
┌─────────────────────────────┐
│         Event Loop          │
│  ready queue │ I/O selectors │
└─────────────────────────────┘
        │ runs coroutine step
        ▼
   await socket.read()
        │ registers interest, suspends
        ▼
   other coroutines run
        │ I/O completes
        ▼
   original coroutine resumes
```

CPython's `asyncio` provides the default loop implementation. One loop per thread is the common pattern. CPU-bound Python code blocks the loop unless offloaded (`asyncio.to_thread`, executors, multiprocessing).

Async does not make Python compute in parallel on multiple cores for pure Python bytecode—it interleaves waiting work efficiently.

# Example

```python
import asyncio

async def main():
    print("start")
    await asyncio.sleep(1)   # yield control to loop
    print("done")

asyncio.run(main())
```

# Common mistakes

- Calling blocking APIs (`time.sleep`, sync `requests.get`) inside async functions without offloading.
- Running multiple `asyncio.run()` calls nested improperly in the same thread.
- Expecting async to speed CPU-bound algorithms without parallelism strategy.
- Mixing threads and async without understanding which loop owns callbacks.

# Related concepts

- [Coroutines](/concepts/coroutines.md)
- [asyncio Basics](/concepts/asyncio_basics.md)
- [await and async Lifecycle](/concepts/await_async_lifecycle.md)
