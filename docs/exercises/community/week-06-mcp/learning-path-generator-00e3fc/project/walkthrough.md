# learning path generator: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Stateful tools: plan and mark steps complete

The source todo exercise keeps descriptions and completion flags. They must stay aligned. User-visible numbering starts at one, while list indices start at zero.

**Ask yourself:** Which list element changes when you complete todo number 2?

**Practice:** Append a new description and its completion flag together. Try an invalid number and explain the error.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Register a real local FastMCP tool without starting a server

The decorator registers a Python function with FastMCP. Tool listing here happens directly in process; it neither launches a subprocess nor calls a hosted model.

**Ask yourself:** Which tool name will be discovered?

**Practice:** Register a second harmless function. To expose stdio later, use a separate script with server.run(transport="stdio"); stdout then carries protocol messages.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [6_mcp/community_contributions/learning_path_generator/data/paths.json](../../../../../../6_mcp/community_contributions/learning_path_generator/data/paths.json): supporting source/configuration; inspect its inputs and effects before execution.
- [6_mcp/community_contributions/learning_path_generator/data/progress.json](../../../../../../6_mcp/community_contributions/learning_path_generator/data/progress.json): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [6_mcp/community_contributions/learning_path_generator/app.py](../../../../../../6_mcp/community_contributions/learning_path_generator/app.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `learning_data` — Local or third-party dependency; follow its definition/import before execution.
- `path_generator` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `format_path_display` | function |
| 61 | `generate_path_async` | async function |
| 118 | `generate_path_sync` | function |
| 123 | `load_path_display` | function |
| 135 | `mark_step_complete_sync` | function |
| 151 | `list_paths` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `path_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `learning_data`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `format_path_display(path_data)`. Its body runs when called.
- Define `generate_path_async(goal, level)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `generate_path_sync(goal, level)`. Its body runs when called.
- Define `load_path_display(path_id)`. Its body runs when called.
- Define `mark_step_complete_sync(path_id, step_id)`. Its body runs when called.
- Define `list_paths()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `path_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `path.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `progress.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `step.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `resource.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/learning_path_generator/learning_data.py](../../../../../../6_mcp/community_contributions/learning_path_generator/learning_data.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `pathlib` — Explicit filesystem paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `load_json_file` | function |
| 28 | `save_json_file` | function |
| 34 | `save_path` | function |
| 41 | `get_path` | function |
| 47 | `get_all_paths` | function |
| 52 | `mark_step_completed` | function |
| 95 | `get_next_step` | function |
| 124 | `get_path_progress` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DATA_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS_FILE`. Re-executing this cell updates kernel state.
- Store expression result in `PROGRESS_FILE`. Re-executing this cell updates kernel state.
- Define `load_json_file(file_path, default)`. Its body runs when called.
- Define `save_json_file(file_path, data)`. Its body runs when called.
- Define `save_path(path)`. Its body runs when called.
- Define `get_path(path_id)`. Its body runs when called.
- Define `get_all_paths()`. Its body runs when called.
- Define `mark_step_completed(path_id, step_id)`. Its body runs when called.
- Define `get_next_step(path_id)`. Its body runs when called.
- Define `get_path_progress(path_id)`. Its body runs when called.
- `paths.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `path.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `progress.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `progress.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `path_progress.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/learning_path_generator/learning_server.py](../../../../../../6_mcp/community_contributions/learning_path_generator/learning_server.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `learning_data` — Local or third-party dependency; follow its definition/import before execution.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `generate_path` | async function |
| 65 | `update_path_content` | async function |
| 153 | `mark_completed` | async function |
| 172 | `get_next_step` | async function |
| 192 | `get_path_details` | async function |
| 220 | `read_path_resource` | async function |

**Read in this order:**

- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `learning_data`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `generate_path(goal, level)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `update_path_content(path_id, steps)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `mark_completed(path_id, step_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_next_step(path_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_path_details(path_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_path_resource(path_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.dumps`: Convert Python data to JSON text.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `progress.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `path.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `step.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/learning_path_generator/path_generator.py](../../../../../../6_mcp/community_contributions/learning_path_generator/path_generator.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `learning_data` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `get_path_generator_mcp_servers` | function |
| 39 | `generate_learning_path` | async function |
| 148 | `test` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store dictionary in `brave_env`. Re-executing this cell updates kernel state.
- Define `get_path_generator_mcp_servers()`. Its body runs when called.
- Define `generate_learning_path(goal, level)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `path.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
