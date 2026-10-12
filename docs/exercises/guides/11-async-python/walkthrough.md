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

## Instructor code, included here

These are the code examples from the local guide, in their original order, so you can compare the instructor's syntax with the explanations above without opening another file. These excerpts can depend on earlier definitions. The complete teaching examples above include their own inputs.

Source examples that intentionally fail, run indefinitely, download software, or contact a service are labeled below. Hosted configuration reads have been changed to your explicit `.environment/.env` path; credential values are never included.

### And now some examples:

Notebook async example: await runs a coroutine; a bare call only creates it. The complete scripts above show where asyncio.run belongs.

```python
# Let's define an async function

import asyncio

async def do_some_work():
    print("Starting work")
    await asyncio.sleep(1)
    print("Work complete")
```

### And now some examples:

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# What will this do?

do_some_work()
```

### And now some examples:

Notebook async example: await runs a coroutine; a bare call only creates it. The complete scripts above show where asyncio.run belongs.

```python
# OK let's try that again!

await do_some_work()
```

### And now some examples:

Notebook async example: await runs a coroutine; a bare call only creates it. The complete scripts above show where asyncio.run belongs.

```python
# What's wrong with this?

async def do_a_lot_of_work():
    do_some_work()
    do_some_work()
    do_some_work()

await do_a_lot_of_work()
```

### And now some examples:

Notebook async example: await runs a coroutine; a bare call only creates it. The complete scripts above show where asyncio.run belongs.

```python
# Interesting warning! Let's fix it

async def do_a_lot_of_work():
    await do_some_work()
    await do_some_work()
    await do_some_work()

await do_a_lot_of_work()
```

### And now some examples:

Notebook async example: await runs a coroutine; a bare call only creates it. The complete scripts above show where asyncio.run belongs.

```python
# And now let's do it in parallel
# It's important to recognize that this is not "multi-threading" in the way that you may be used to
# The asyncio library is running on a single thread, but it's using a loop to switch between tasks while one is waiting

async def do_a_lot_of_work_in_parallel():
    await asyncio.gather(do_some_work(), do_some_work(), do_some_work())

await do_a_lot_of_work_in_parallel()
```

### Finally - try writing a python module that calls do_a_lot_of_work_in_parallel

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
if __name__ == "__main__":
    asyncio.run(do_a_lot_of_work_in_parallel())
```

---

Source: [11_async_python.ipynb](../../../../guides/11_async_python.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
