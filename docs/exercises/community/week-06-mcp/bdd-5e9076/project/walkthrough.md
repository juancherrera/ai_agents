# BDD: project walkthrough

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

## Configuration and supporting files

- [6_mcp/community_contributions/BDD/readme.md](../../../../../../6_mcp/community_contributions/BDD/readme.md): BDD Agent System with MCP Servers. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [6_mcp/community_contributions/BDD/test-results/reports/Todo_App_Management_20251027_195007_report.json](../../../../../../6_mcp/community_contributions/BDD/test-results/reports/Todo_App_Management_20251027_195007_report.json): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [6_mcp/community_contributions/BDD/bddAgent.py](../../../../../../6_mcp/community_contributions/BDD/bddAgent.py)

**Imports:**

- `IPython.display` — Notebook display/shell facilities.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `anyio` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 102 | `BDDAgent` | class |
| 181 | `ignore_anyio_cancel_error` | function |
| 187 | `load_bdd_prompt` | function |
| 195 | `main` | async function |
| 104 | `__init__` | function |
| 111 | `getBddAgent` | async function |
| 122 | `run` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`, `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anyio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `current_dir`. Re-executing this cell updates kernel state.
- Store expression result in `test_results_dir`. Re-executing this cell updates kernel state.
- Store expression result in `reports_dir`. Re-executing this cell updates kernel state.
- Store expression result in `screenshots_dir`. Re-executing this cell updates kernel state.
- Store formatted string in `system_prompt`. Re-executing this cell updates kernel state.
- Store list in `mcp_server_params`. Re-executing this cell updates kernel state.
- Define class `BDDAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define `ignore_anyio_cancel_error(loop, context)`. Its body runs when called.
- Define `load_bdd_prompt(prompt_path)`. Its body runs when called.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `context.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `mcp_servers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/BDD/mcp-servers/assertion-server/assertion_server_python.py](../../../../../../6_mcp/community_contributions/BDD/mcp-servers/assertion-server/assertion_server_python.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `json` — Encoding/decoding structured text.
- `mcp.server` — Tool/resource protocol, clients, and servers.
- `mcp.server.stdio` — Tool/resource protocol, clients, and servers.
- `mcp.types` — Tool/resource protocol, clients, and servers.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 22 | `list_tools` | async function |
| 227 | `call_tool` | async function |
| 407 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.stdio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.types`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Define `list_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `call_tool(name, arguments)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `app.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `app.call_tool`: Invoke a tool with named arguments while its session is connected.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `arguments.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `app.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.dumps`: Convert Python data to JSON text.
- `re.compile`: Build an executable graph or compiled object; this is separate from running it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
