# 11 · Understand async by comparing waiting patterns

Async lets a program do other work while an operation is waiting. It is useful when several model or tool requests spend time waiting for responses. It does not automatically make CPU calculations faster.

## A complete script: wait for one operation

```python
import asyncio

async def work(name):
    print("Start", name)
    await asyncio.sleep(0.01)  # Simulate waiting without blocking the event loop.
    print("End", name)
    return name.upper()

async def main():
    result = await work("one")
    print(result)

if __name__ == "__main__":
    asyncio.run(main())
```

Output: `Start one`, `End one`, then `ONE`. `async def` defines a coroutine function. Calling `work("one")` creates a coroutine; `await` runs it and waits for its result while allowing other scheduled work to progress. `asyncio.run` starts an event loop for a normal script.

In a notebook, use the same definitions but replace the final `if` block with `await main()`. Notebooks already have an event loop, so starting another with `asyncio.run` can fail there.

## Two awaits in sequence still run sequentially

```python
import asyncio

async def work(name):
    print("Start", name)
    await asyncio.sleep(0.01)
    print("End", name)
    return name.upper()

async def main():
    first = await work("one")
    second = await work("two")
    print([first, second])

if __name__ == "__main__":
    asyncio.run(main())
```

The order is `Start one`, `End one`, `Start two`, `End two`, then `['ONE', 'TWO']`. The second call is not reached until the first await finishes. Adding `async` alone does not schedule concurrent work.

## Gather schedules both operations

```python
import asyncio

async def work(name):
    print("Start", name)
    await asyncio.sleep(0.01)
    print("End", name)
    return name.upper()

async def main():
    results = await asyncio.gather(work("one"), work("two"))
    print(results)

if __name__ == "__main__":
    asyncio.run(main())
```

Both start before either finishes waiting. The final result is `['ONE', 'TWO']`: `gather` returns values in argument order, even if operations finish in a different order. Completion order generally depends on timing. Use this for independent requests; if the second requires the first result, sequential awaits are appropriate.

## Common mistakes in the source examples

Calling an async function without awaiting or scheduling its coroutine does not execute its body and can produce an unawaited-coroutine warning. Forgetting `await` on `asyncio.sleep` removes the intended wait. Using `time.sleep` inside async code blocks the event loop, preventing other coroutines from progressing during that sleep.

## Practice

Add `work("three")` to `gather`. Expected final list: `['ONE', 'TWO', 'THREE']`. Then replace `gather` with three sequential awaits and observe the difference in the start/end messages. Do not infer a performance guarantee from this tiny timing simulation.

---

Source: [11_async_python.ipynb](../../../../guides/11_async_python.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
