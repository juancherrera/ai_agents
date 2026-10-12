# kachaje andela genai bootcamp: project walkthrough

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

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

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

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Simulated accounts change state

Buying shares updates cash, holdings, and a transaction record. Practice against a dictionary so you do not import account modules that create or alter databases.

**Ask yourself:** How much cash remains after buying three fictional shares at 100 each?

**Practice:** Try an unaffordable order and confirm the balance is unchanged. Explain why reset is a data-changing action.

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

## Configuration and supporting files

- [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/app.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/app.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `pandas` — Tabular data.
- `plotly.express` — Charts.
- `setup` — Local or third-party dependency; follow its definition/import before execution.
- `shared` — Local or third-party dependency; follow its definition/import before execution.
- `trading_floor` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `Trader` | class |
| 96 | `TraderView` | class |
| 168 | `create_ui` | function |
| 20 | `__init__` | function |
| 26 | `reload` | function |
| 29 | `get_title` | function |
| 32 | `get_strategy` | function |
| 35 | `get_portfolio_value_df` | function |
| 40 | `get_portfolio_value_chart` | function |
| 56 | `get_holdings_df` | function |
| 67 | `get_transactions_df` | function |
| 75 | `get_portfolio_value` | function |
| 83 | `get_logs` | function |
| 97 | `__init__` | function |
| 104 | `make_ui` | function |
| 158 | `refresh` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shared`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `trading_floor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `plotly.express`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `setup`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store dictionary in `mapper`. Re-executing this cell updates kernel state.
- Define class `Trader`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TraderView`. Separate the class definition, constructing an instance, and calling its methods.
- Define `create_ui()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `mapper.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/__init__.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/__init__.py)

**Imports:**

- `.trace` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.trace`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/trace.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/trace.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `dataclasses` — Simple typed records.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `pathlib` — Explicit filesystem paths.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `Span` | class |
| 56 | `_get_current_span` | function |
| 61 | `_set_current_span` | function |
| 66 | `_get_log_file_path` | function |
| 74 | `_get_json_log_file_path` | function |
| 82 | `_write_log` | function |
| 94 | `_write_json_log` | function |
| 105 | `_extract_metadata_from_result` | function |
| 190 | `LocalTraceProcessor` | class |
| 389 | `_get_local_trace_processor` | function |
| 400 | `span` | function |
| 460 | `local_trace` | function |
| 45 | `to_dict` | function |
| 50 | `add_result_metadata` | function |
| 199 | `__init__` | function |
| 204 | `_get_span_map` | function |
| 210 | `_extract_span_name` | function |
| 234 | `_extract_span_attributes` | function |
| 267 | `on_span_start` | function |
| 306 | `on_span_end` | function |
| 349 | `on_trace_start` | function |
| 354 | `on_trace_end` | function |
| 374 | `force_flush` | function |
| 378 | `shutdown` | function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `_thread_local`. Re-executing this cell updates kernel state.
- Store dictionary in `_thread_local.span_map`. Re-executing this cell updates kernel state.
- Define class `Span`. Separate the class definition, constructing an instance, and calling its methods.
- Define `_get_current_span()`. Its body runs when called.
- Define `_set_current_span(span)`. Its body runs when called.
- Define `_get_log_file_path()`. Its body runs when called.
- Define `_get_json_log_file_path()`. Its body runs when called.
- Define `_write_log(message)`. Its body runs when called.
- Define `_write_json_log(span)`. Its body runs when called.
- Define `_extract_metadata_from_result(result)`. Its body runs when called.
- Define class `LocalTraceProcessor`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `_local_trace_processor`. Re-executing this cell updates kernel state.
- Store expression result in `_processor_lock`. Re-executing this cell updates kernel state.
- Define `_get_local_trace_processor()`. Its body runs when called.
- Define `span(name, trace_id, attributes)`. Its body runs when called.
- Define `local_trace(name, trace_id, attributes)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `local_span_info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `msg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tool_calls.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.
- `to_remove.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tc.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `usage.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/trace_ui.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/local_trace/trace_ui.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `flask` — Local or third-party dependency; follow its definition/import before execution.
- `glob` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `pathlib` — Explicit filesystem paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `load_traces` | function |
| 71 | `_build_span_hierarchy` | function |
| 90 | `_format_timestamp` | function |
| 445 | `index` | function |
| 451 | `api_traces` | function |
| 458 | `api_trace` | function |

**Read in this order:**

- Make available: `flask`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `glob`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Define `load_traces()`. Its body runs when called.
- Define `_build_span_hierarchy(spans)`. Its body runs when called.
- Define `_format_timestamp(iso_str)`. Its body runs when called.
- Store expression result in `HTML_TEMPLATE`. Re-executing this cell updates kernel state.
- Define `index()`. Its body runs when called.
- Define `api_traces()`. Its body runs when called.
- Define `api_trace(trace_id)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `app.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `span.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `root_spans.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `x.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `span_data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/mcp_params.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/mcp_params.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `market` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 35 | `researcher_mcp_server_params` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store dictionary in `brave_env`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `trader_mcp_server_params`. Re-executing this cell updates kernel state.
- Store dictionary in `local_web_search_mcp`. Re-executing this cell updates kernel state.
- Store dictionary in `brave_search_mcp`. Re-executing this cell updates kernel state.
- Define `researcher_mcp_server_params(name, use_local_search)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/setup.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/setup.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `init` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `init()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/shared.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/shared.py)

**Imports:**

- `database` — Local or third-party dependency; follow its definition/import before execution.
- `market` — Local or third-party dependency; follow its definition/import before execution.
- `market_server` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `push_server` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `templates` — Local or third-party dependency; follow its definition/import before execution.
- `tracers` — Local or third-party dependency; follow its definition/import before execution.
- `util` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `_current_dir`. Re-executing this cell updates kernel state.
- Store expression result in `_root_dir`. Re-executing this cell updates kernel state.
- Store expression result in `_current_dir_str`. Re-executing this cell updates kernel state.
- Store expression result in `_root_dir_str`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Make available: `database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `market_server`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `push_server`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `util`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.
- `sys.path.remove`: Delete an item/file depending on the receiver. Check the target.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/traders.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/traders.py)

**Imports:**

- `accounts_client` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `importlib.util` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `mcp_params` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `pathlib` — Explicit filesystem paths.
- `shared` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 36 | `get_model` | function |
| 42 | `get_researcher` | async function |
| 52 | `get_researcher_tool` | async function |
| 57 | `Trader` | class |
| 58 | `__init__` | function |
| 65 | `create_agent` | async function |
| 76 | `get_account_report` | async function |
| 82 | `run_agent` | async function |
| 93 | `run_with_mcp_servers` | async function |
| 110 | `run_with_trace` | async function |
| 116 | `run` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shared`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts_client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp_params`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `importlib.util`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `trace_path`. Re-executing this cell updates kernel state.
- Store expression result in `spec`. Re-executing this cell updates kernel state.
- Store expression result in `local_trace_module`. Re-executing this cell updates kernel state.
- Store expression result in `local_trace`. Re-executing this cell updates kernel state.
- Store expression result in `OLLAMA_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_client`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_TURNS`. Re-executing this cell updates kernel state.
- Define `get_model(model_name)`. Its body runs when called.
- Define `get_researcher(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_researcher_tool(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define class `Trader`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `researcher.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json.dumps`: Convert Python data to JSON text.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/trading_floor.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/trading_floor.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `setup` — Local or third-party dependency; follow its definition/import before execution.
- `shared` — Local or third-party dependency; follow its definition/import before execution.
- `traders` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 34 | `create_traders` | function |
| 41 | `run_every_n_minutes` | async function |

**Read in this order:**

- Make available: `traders`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shared`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `setup`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `RUN_EVERY_N_MINUTES`. Re-executing this cell updates kernel state.
- Store expression result in `RUN_EVEN_WHEN_MARKET_IS_CLOSED`. Re-executing this cell updates kernel state.
- Store expression result in `USE_MANY_MODELS`. Re-executing this cell updates kernel state.
- Store list in `names`. Re-executing this cell updates kernel state.
- Store list in `lastnames`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define `create_traders()`. Its body runs when called.
- Define `run_every_n_minutes()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `traders.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `trader.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/kachaje-andela-genai-bootcamp/web_search_server.py](../../../../../../6_mcp/community_contributions/kachaje-andela-genai-bootcamp/web_search_server.py)

**Imports:**

- `ddgs` — Local or third-party dependency; follow its definition/import before execution.
- `ddgs.exceptions` — Local or third-party dependency; follow its definition/import before execution.
- `local_trace` — Local or third-party dependency; follow its definition/import before execution.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `SearchResult` | class |
| 19 | `search_web` | async function |

**Read in this order:**

- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ddgs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ddgs.exceptions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `local_trace`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define class `SearchResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `search_web(query, max_results)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `Field`: Describe a schema field or constrain allowed values.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
