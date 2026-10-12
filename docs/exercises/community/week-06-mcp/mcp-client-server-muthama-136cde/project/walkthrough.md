# mcp client server muthama: project walkthrough

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

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

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

## Module-by-module reading map

### [6_mcp/community_contributions/mcp_client_server_muthama/agent.py](../../../../../../6_mcp/community_contributions/mcp_client_server_muthama/agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 23 | `run_date_agent` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OPENAI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `DEFAULT_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define `run_date_agent(user_query, client_session_timeout_seconds)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/mcp_client_server_muthama/gradio_ui.py](../../../../../../6_mcp/community_contributions/mcp_client_server_muthama/gradio_ui.py)

**Imports:**

- `agent` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `build_ui` | function |
| 23 | `respond` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `build_ui()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.
- `chat_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `agent_reply.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/mcp_client_server_muthama/server.py](../../../../../../6_mcp/community_contributions/mcp_client_server_muthama/server.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `current_date` | function |
| 14 | `current_time` | function |
| 20 | `shift_date` | function |
| 28 | `days_between` | function |
| 36 | `weekday_of` | function |
| 43 | `day_of_week` | function |
| 50 | `iso_week_number` | function |
| 58 | `format_date` | function |
| 65 | `to_timestamp` | function |
| 74 | `from_timestamp` | function |
| 81 | `next_weekday` | function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp_server`. Re-executing this cell updates kernel state.
- Define `current_date()`. Its body runs when called.
- Define `current_time()`. Its body runs when called.
- Define `shift_date(base_date, days)`. Its body runs when called.
- Define `days_between(start_date, end_date)`. Its body runs when called.
- Define `weekday_of(date_str)`. Its body runs when called.
- Define `day_of_week(date_str)`. Its body runs when called.
- Define `iso_week_number(date_str)`. Its body runs when called.
- Define `format_date(date_str, fmt)`. Its body runs when called.
- Define `to_timestamp(date_str)`. Its body runs when called.
- Define `from_timestamp(ts)`. Its body runs when called.
- Define `next_weekday(date_str, weekday)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp_server.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp_server.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
