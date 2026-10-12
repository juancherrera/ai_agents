# simple mcp date server: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Register a real local FastMCP tool without starting a server

The decorator registers a Python function with FastMCP. Tool listing here happens directly in process; it neither launches a subprocess nor calls a hosted model.

**Ask yourself:** Which tool name will be discovered?

**Practice:** Register a second harmless function. To expose stdio later, use a separate script with server.run(transport="stdio"); stdout then carries protocol messages.

## Configuration and supporting files

- [6_mcp/community_contributions/simple_mcp_date_server/README.md](../../../../../../6_mcp/community_contributions/simple_mcp_date_server/README.md): Simple date server. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [6_mcp/community_contributions/simple_mcp_date_server/server.py](../../../../../../6_mcp/community_contributions/simple_mcp_date_server/server.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `current_date` | async function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `current_date()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/simple_mcp_date_server/simple_client.py](../../../../../../6_mcp/community_contributions/simple_mcp_date_server/simple_client.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `mcp` — Tool/resource protocol, clients, and servers.
- `mcp.client.stdio` — Tool/resource protocol, clients, and servers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.client.stdio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `session.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `session.call_tool`: Invoke a tool with named arguments while its session is connected.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/simple_mcp_date_server/simple_client_llm.py](../../../../../../6_mcp/community_contributions/simple_mcp_date_server/simple_client_llm.py)

**Imports:**

- `anthropic` — Native Claude client.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `mcp` — Tool/resource protocol, clients, and servers.
- `mcp.client.stdio` — Tool/resource protocol, clients, and servers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.client.stdio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `anthropic.Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `client.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `session.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `tools.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tool_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `session.call_tool`: Invoke a tool with named arguments while its session is connected.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
