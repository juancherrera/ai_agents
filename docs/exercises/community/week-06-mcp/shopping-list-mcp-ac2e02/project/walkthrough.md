# shopping list mcp: project walkthrough

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Register a real local FastMCP tool without starting a server

The decorator registers a Python function with FastMCP. Tool listing here happens directly in process; it neither launches a subprocess nor calls a hosted model.

**Ask yourself:** Which tool name will be discovered?

**Practice:** Register a second harmless function. To expose stdio later, use a separate script with server.run(transport="stdio"); stdout then carries protocol messages.

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

## Configuration and supporting files

- [6_mcp/community_contributions/shopping_list_mcp/README.md](../../../../../../6_mcp/community_contributions/shopping_list_mcp/README.md): 🛒 Shopping List MCP Server. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [6_mcp/community_contributions/shopping_list_mcp/email_server.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/email_server.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `resend` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `find_and_load_dotenv` | function |
| 42 | `log` | function |
| 52 | `init_resend` | function |
| 61 | `send_email` | async function |
| 145 | `send_shopping_list_email` | async function |

**Read in this order:**

- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resend`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `find_and_load_dotenv()`. Its body runs when called.
- Store expression result in `env_path`. Re-executing this cell updates kernel state.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `log(msg)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define `init_resend()`. Its body runs when called.
- Define `send_email(to_email, subject, body, from_email)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `send_shopping_list_email(to_email, items, total_cost, budget, note)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `email.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/shopping_list_mcp/server.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/server.py)

**Imports:**

- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `shopping_list` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `add_item` | async function |
| 31 | `remove_item` | async function |
| 43 | `get_list` | async function |
| 51 | `set_budget` | async function |
| 61 | `get_budget_status` | async function |
| 73 | `clear_list` | async function |

**Read in this order:**

- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shopping_list`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `add_item(name, quantity, category, price)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `remove_item(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_list()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `set_budget(amount)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_budget_status()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `clear_list()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/shopping_list_mcp/shopping_agent.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/shopping_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 42 | `get_llm_model` | function |
| 77 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `GROQ_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GEMINI_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Define `get_llm_model()`. Its body runs when called.
- Store expression result in `node_bin_path`. Re-executing this cell updates kernel state.
- Store expression result in `npx_path`. Re-executing this cell updates kernel state.
- Store expression result in `uv_path`. Re-executing this cell updates kernel state.
- Store expression result in `node_env`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `node_env.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `mcp_servers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `shopping_server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `memory_server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `conversation_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `email_server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `sms_server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/shopping_list_mcp/shopping_list.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/shopping_list.py)

**Imports:**

- `dataclasses` — Simple typed records.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `ShoppingItem` | class |
| 22 | `ShoppingList` | class |
| 220 | `get_shopping_list` | function |
| 29 | `__init__` | function |
| 37 | `add_item` | function |
| 71 | `remove_item` | function |
| 87 | `get_list` | function |
| 101 | `clear_list` | function |
| 113 | `set_budget` | function |
| 122 | `get_total_cost` | function |
| 129 | `get_budget_status` | function |
| 156 | `_item_to_dict` | function |
| 170 | `format_for_sms` | function |
| 186 | `format_for_email` | function |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ShoppingItem`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ShoppingList`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `_shopping_list_instance`. Re-executing this cell updates kernel state.
- Define `get_shopping_list()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/shopping_list_mcp/simple_client.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/simple_client.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `mcp` — Tool/resource protocol, clients, and servers.
- `mcp.client.stdio` — Tool/resource protocol, clients, and servers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `main` | async function |

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

### [6_mcp/community_contributions/shopping_list_mcp/sms_server.py](../../../../../../6_mcp/community_contributions/shopping_list_mcp/sms_server.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `resend` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 27 | `find_and_load_dotenv` | function |
| 73 | `init_resend` | function |
| 81 | `clean_phone_number` | function |
| 91 | `get_sms_email` | function |
| 104 | `send_sms` | async function |
| 163 | `list_supported_carriers` | async function |
| 185 | `send_shopping_list_sms` | async function |

**Read in this order:**

- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resend`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `find_and_load_dotenv()`. Its body runs when called.
- Store expression result in `env_path`. Re-executing this cell updates kernel state.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Store dictionary in `CARRIER_GATEWAYS`. Re-executing this cell updates kernel state.
- Define `init_resend()`. Its body runs when called.
- Define `clean_phone_number(phone)`. Its body runs when called.
- Define `get_sms_email(phone, carrier)`. Its body runs when called.
- Define `send_sms(phone_number, carrier, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `list_supported_carriers()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `send_shopping_list_sms(phone_number, carrier, items, total_cost)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `CARRIER_GATEWAYS.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `email.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
