# Async Python and streaming

Sources: [async guide](../guides/11_async_python.ipynb), [OpenAI workflow](../2_openai/2_lab2.ipynb), [LangGraph async lesson](../4_langgraph/3_lab3.ipynb).

## A complete script

```python
import asyncio

async def work(name: str) -> str:
    await asyncio.sleep(0.01)
    return f"Finished {name}"

async def main():
    results = await asyncio.gather(work("A"), work("B"))
    print(results)

if __name__ == "__main__":
    asyncio.run(main())
```

Expected output: `['Finished A', 'Finished B']`.

`async def` defines a coroutine. Calling `work("A")` creates a coroutine object; `await` executes it and waits for its result. `asyncio.gather` schedules multiple coroutines concurrently and returns results in input order. This helps with waiting on independent API calls; it also means several requests can consume API budget at once.

In a notebook, use `await main()` instead of `asyncio.run(main())`, because the notebook already has an event loop.

## Streaming

The OpenAI SDK lesson uses `Runner.run_streamed(...)`, then an `async for` loop over `result.stream_events()` to display output as it arrives. A stream can include several event types; the lesson filters for `ResponseTextDeltaEvent` before reading `event.data.delta`.

The UI/research modules use `yield` to send progress updates. Async generators are consumed with `async for`; ordinary generators with `for`.

## Common mistakes

| Symptom | Check |
| --- | --- |
| Coroutine was never awaited | Did you use `await` or schedule the coroutine? |
| `asyncio.run()` cannot be called from a running loop | Use notebook top-level `await` |
| Async code still blocks | Look for synchronous calls such as `time.sleep` or `requests.get` inside async functions |
| Tool / graph invocation differs | Course LangGraph uses `tool.run` / `await tool.arun` and `graph.invoke` / `await graph.ainvoke` |

Use `async with` for resources whose setup and cleanup are asynchronous, such as the course's MCP server connections.
