# agent test

Source: [6_mcp/community_contributions/BDD/agent_test.ipynb](../../../../../../../6_mcp/community_contributions/BDD/agent_test.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Messages → An agent loop must make progress and stop → Await vs. concurrent work → Browser tools have setup, action, and cleanup → MCP tools, resources, and server lifetime

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

## Follow the source in order

### Cell 1: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`, `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: Test the complete BDD agent with all 3 MCP servers:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Store expression result in `current_dir`. Re-executing this cell updates kernel state.
- Store expression result in `test_results_dir`. Re-executing this cell updates kernel state.
- Store expression result in `reports_dir`. Re-executing this cell updates kernel state.
- Store expression result in `screenshots_dir`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `gherkin_feature`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store formatted string in `system_prompt`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

Empty code cell; it defines no state.

### Cell 7: code

- Store list in `mcp_server_params`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store dictionary in `temp_param`. Re-executing this cell updates kernel state.
- Store expression result in `server`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `server.connect`: Open a database or network/server connection; identify the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Define `getBddAgent(mcp_servers)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store list in `mcp_servers`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `bddAgent`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `mcp_servers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 10 | `getBddAgent` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
