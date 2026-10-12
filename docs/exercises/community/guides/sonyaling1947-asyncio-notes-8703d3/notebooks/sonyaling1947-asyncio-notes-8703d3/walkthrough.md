# sonyaling1947 asyncio notes

Source: [guides/community_contributions/sonyaling1947_asyncio_notes.ipynb](../../../../../../../guides/community_contributions/sonyaling1947_asyncio_notes.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

An agent loop must make progress and stop → Await vs. concurrent work

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

## Follow the source in order

### Cell 1: Async Python

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: And now some examples:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `do_some_work(name, secs)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Define `do_a_lot_of_work()`. Its body runs when called; await/schedule the coroutine to execute async work.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define `do_a_lot_of_work()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `begin`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `begin`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `sync_work_with_time_sleep(name, secs)`. Its body runs when called.
- Store expression result in `begin`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: Finally - try writing a python module that calls do_a_lot_of_work_in_parallel

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: blank separator

No execution needed.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 3 | `do_some_work` | async function |
| 6 | `do_a_lot_of_work` | async function |
| 7 | `do_a_lot_of_work` | async function |
| 9 | `sync_work_with_time_sleep` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
