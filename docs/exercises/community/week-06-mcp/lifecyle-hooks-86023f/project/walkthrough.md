# lifecyle hooks: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

## Configuration and supporting files

- [6_mcp/community_contributions/lifecyle_hooks/sandbox/recipe.md](../../../../../../6_mcp/community_contributions/lifecyle_hooks/sandbox/recipe.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [6_mcp/community_contributions/lifecyle_hooks/agents_logging.py](../../../../../../6_mcp/community_contributions/lifecyle_hooks/agents_logging.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `AgentLoggingHooks` | class |
| 7 | `__init__` | function |
| 11 | `_get_timestamp` | function |
| 15 | `on_start` | async function |
| 20 | `on_end` | async function |
| 27 | `on_handoff` | async function |
| 34 | `on_tool_start` | async function |
| 41 | `on_tool_end` | async function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AgentLoggingHooks` using `AgentHooks`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/lifecyle_hooks/mcp_server_manager.py](../../../../../../6_mcp/community_contributions/lifecyle_hooks/mcp_server_manager.py)

**Imports:**

- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `MCPServers` | class |
| 6 | `__init__` | function |
| 17 | `__aenter__` | async function |
| 31 | `__aexit__` | async function |
| 35 | `__getattr__` | function |
| 41 | `get_all_servers` | function |
| 45 | `get_servers_by_names` | function |
| 49 | `get_server_dict` | function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MCPServers`. Separate the class definition, constructing an instance, and calling its methods.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `config.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
