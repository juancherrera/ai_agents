# iamumarjaved: project walkthrough

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [6_mcp/community_contributions/iamumarjaved/app.py](../../../../../../6_mcp/community_contributions/iamumarjaved/app.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `os` — Process configuration and operating-system paths.
- `src.ui.app` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `trading_floor` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `run_trading_floor_in_background` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.ui.app`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `trading_floor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run_trading_floor_in_background()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `trading_thread.start`: Start the receiver runtime/server/resource.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/reset.py](../../../../../../6_mcp/community_contributions/iamumarjaved/reset.py)

**Imports:**

- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 41 | `reset_traders` | function |

**Read in this order:**

- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `waren_strategy`. Re-executing this cell updates kernel state.
- Store expression result in `george_strategy`. Re-executing this cell updates kernel state.
- Store expression result in `ray_strategy`. Re-executing this cell updates kernel state.
- Store expression result in `cathie_strategy`. Re-executing this cell updates kernel state.
- Define `reset_traders()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `Account.get(...).reset`: Reset state. For accounts, this can replace stored data.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/agents/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/agents/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/agents/accounts.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/agents/accounts.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `pydantic` — Validated structured objects.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.market` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `Transaction` | class |
| 27 | `Account` | class |
| 21 | `total` | function |
| 24 | `__repr__` | function |
| 37 | `get` | function |
| 52 | `save` | function |
| 55 | `reset` | function |
| 63 | `deposit` | function |
| 70 | `withdraw` | function |
| 77 | `can_trade_now` | function |
| 91 | `buy_shares` | function |
| 113 | `sell_shares` | function |
| 136 | `calculate_portfolio_value` | function |
| 142 | `calculate_profit_loss` | function |
| 146 | `get_holdings` | function |
| 149 | `get_profit_loss` | function |
| 152 | `list_transactions` | function |
| 155 | `report` | function |
| 166 | `get_strategy` | function |
| 170 | `change_strategy` | function |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INITIAL_BALANCE`. Re-executing this cell updates kernel state.
- Store expression result in `SPREAD`. Re-executing this cell updates kernel state.
- Store expression result in `MIN_MINUTES_BETWEEN_TRADES`. Re-executing this cell updates kernel state.
- Define class `Transaction` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Account` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `account.sell_shares`: Change the simulated account holdings, cash, and transaction records.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.portfolio_value_time_series.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.
- `self.holdings.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/agents/news_sentinel.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/agents/news_sentinel.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `json` — Encoding/decoding structured text.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.mcp_params` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.tracers` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `NewsSentinel` | class |
| 11 | `__init__` | function |
| 16 | `get_all_holdings` | function |
| 25 | `get_unique_symbols` | function |
| 31 | `create_agent` | async function |
| 65 | `run_agent` | async function |
| 100 | `run_with_mcp_servers` | async function |
| 113 | `run_with_trace` | async function |
| 118 | `run` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `NewsSentinel`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `json.dumps`: Convert Python data to JSON text.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/agents/risk_manager.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/agents/risk_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.mcp_params` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.templates` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.tracers` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `RiskManager` | class |
| 19 | `__init__` | function |
| 24 | `create_agent` | async function |
| 33 | `gather_all_accounts_data` | function |
| 43 | `run_agent` | async function |
| 50 | `run_with_mcp_servers` | async function |
| 61 | `run_with_trace` | async function |
| 68 | `run` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.mcp_params`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MAX_TURNS`. Re-executing this cell updates kernel state.
- Define class `RiskManager`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `json.dumps`: Convert Python data to JSON text.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/agents/traders.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/agents/traders.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.mcp_params` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.templates` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.tracers` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 38 | `get_model` | function |
| 50 | `get_researcher` | async function |
| 59 | `get_researcher_tool` | async function |
| 63 | `Trader` | class |
| 64 | `__init__` | function |
| 71 | `create_agent` | async function |
| 82 | `get_account_report` | async function |
| 88 | `run_agent` | async function |
| 99 | `run_with_mcp_servers` | async function |
| 116 | `run_with_trace` | async function |
| 122 | `run` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.mcp_params`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `grok_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `DEEPSEEK_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GROK_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GEMINI_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `OPENROUTER_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_TURNS`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_client`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_client`. Re-executing this cell updates kernel state.
- Store expression result in `grok_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Define `get_model(model_name)`. Its body runs when called.
- Define `get_researcher(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_researcher_tool(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define class `Trader`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `researcher.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json.dumps`: Convert Python data to JSON text.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/database/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/database/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/database/database.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/database/database.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `sqlite3` — Local database storage.

| Source line | Definition | Kind |
| --- | --- | --- |
| 54 | `write_account` | function |
| 65 | `read_account` | function |
| 72 | `write_log` | function |
| 91 | `read_log` | function |
| 113 | `write_market` | function |
| 124 | `read_market` | function |
| 131 | `write_company_insight` | function |
| 140 | `read_company_insights` | function |
| 150 | `read_trader_company_insights` | function |
| 161 | `write_risk_assessment` | function |
| 170 | `read_latest_risk_assessment` | function |
| 180 | `read_all_risk_assessments` | function |
| 190 | `write_news_alert` | function |
| 199 | `read_latest_news_alerts` | function |
| 209 | `get_connection` | function |

**Read in this order:**

- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DB`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Define `write_account(name, account_dict)`. Its body runs when called.
- Define `read_account(name)`. Its body runs when called.
- Define `write_log(name, type, message)`. Its body runs when called.
- Define `read_log(name, last_n)`. Its body runs when called.
- Define `write_market(date, data)`. Its body runs when called.
- Define `read_market(date)`. Its body runs when called.
- Define `write_company_insight(trader_name, symbol, rationale, action)`. Its body runs when called.
- Define `read_company_insights(symbol)`. Its body runs when called.
- Define `read_trader_company_insights(trader_name, symbol)`. Its body runs when called.
- Define `write_risk_assessment(assessment, recommendations)`. Its body runs when called.
- Define `read_latest_risk_assessment()`. Its body runs when called.
- Define `read_all_risk_assessments(last_n)`. Its body runs when called.
- Define `write_news_alert(symbol, headline, sentiment, affected_traders)`. Its body runs when called.
- Define `read_latest_news_alerts(last_n)`. Its body runs when called.
- Define `get_connection()`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `json.dumps`: Convert Python data to JSON text.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/accounts_client.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/accounts_client.py)

**Imports:**

- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `src.utils.mcp_params` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `get_accounts_tools_openai` | async function |
| 10 | `list_accounts_tools` | async function |
| 16 | `read_accounts_resource` | async function |
| 26 | `read_strategy_resource` | async function |

**Read in this order:**

- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.mcp_params`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_accounts_tools_openai()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `list_accounts_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_accounts_resource(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_strategy_resource(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `server.read_resource`: Read an MCP resource as context, rather than executing a tool.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/accounts_server.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/accounts_server.py)

**Imports:**

- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `get_balance` | async function |
| 27 | `get_holdings` | async function |
| 43 | `check_trading_cooldown` | async function |
| 64 | `buy_shares` | async function |
| 83 | `sell_shares` | async function |
| 102 | `change_strategy` | async function |
| 119 | `read_account_resource` | async function |
| 136 | `read_strategy_resource` | async function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `get_balance(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_holdings(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `check_trading_cooldown(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `buy_shares(name, symbol, quantity, rationale)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `sell_shares(name, symbol, quantity, rationale)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `change_strategy(name, strategy)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_account_resource(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_strategy_resource(name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Account.get(...).buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `Account.get(...).sell_shares`: Change the simulated account holdings, cash, and transaction records.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/market_server.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/market_server.py)

**Imports:**

- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `src.utils.market` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `get_share_price_tool` | async function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `get_share_price_tool(symbol)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/news_server.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/news_server.py)

**Imports:**

- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `record_news_alert` | async function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `record_news_alert(symbol, headline, sentiment, affected_traders)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/mcp_servers/push_server.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/mcp_servers/push_server.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `send_push_notification` | async function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `send_push_notification(name, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/ui/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/ui/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/ui/app.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/ui/app.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `os` — Process configuration and operating-system paths.
- `pandas` — Tabular data.
- `plotly.express` — Charts.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.company_insights` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.util` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 36 | `Trader` | class |
| 109 | `TraderView` | class |
| 180 | `RiskManagerView` | class |
| 233 | `create_ui` | function |
| 37 | `__init__` | function |
| 43 | `reload` | function |
| 46 | `get_title` | function |
| 49 | `get_strategy` | function |
| 52 | `get_portfolio_value_df` | function |
| 57 | `get_portfolio_value_chart` | function |
| 73 | `get_holdings_df` | function |
| 83 | `get_transactions_df` | function |
| 90 | `get_portfolio_value` | function |
| 97 | `get_logs` | function |
| 110 | `__init__` | function |
| 117 | `make_ui` | function |
| 171 | `refresh` | function |
| 181 | `__init__` | function |
| 184 | `get_risk_assessment` | function |
| 196 | `get_logs` | function |
| 208 | `make_ui` | function |
| 265 | `show_deep_dive` | function |
| 278 | `get_active_symbols` | function |
| 311 | `get_news_alerts_display` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.util`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `plotly.express`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.company_insights`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `names`. Re-executing this cell updates kernel state.
- Store list in `lastnames`. Re-executing this cell updates kernel state.
- Store expression result in `USE_MANY_MODELS`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store dictionary in `mapper`. Re-executing this cell updates kernel state.
- Define class `Trader`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TraderView`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RiskManagerView`. Separate the class definition, constructing an instance, and calling its methods.
- Define `create_ui()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `mapper.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/__init__.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/company_insights.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/company_insights.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `get_company_deep_dive` | function |
| 41 | `format_company_deep_dive` | function |
| 59 | `get_all_active_symbols` | function |
| 67 | `get_trader_symbol_insight` | function |

**Read in this order:**

- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_company_deep_dive(symbol)`. Its body runs when called.
- Define `format_company_deep_dive(symbol)`. Its body runs when called.
- Define `get_all_active_symbols()`. Its body runs when called.
- Define `get_trader_symbol_insight(trader_name, symbol)`. Its body runs when called.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `traders_data.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/market.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/market.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `functools` — Caching and function utilities.
- `os` — Process configuration and operating-system paths.
- `polygon` — Market-data API client.
- `random` — Demo/simulation randomness.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `is_market_open` | function |
| 23 | `get_all_share_prices_polygon_eod` | function |
| 33 | `get_market_for_prior_date` | function |
| 40 | `get_share_price_polygon_eod` | function |
| 45 | `get_share_price_polygon_min` | function |
| 50 | `get_share_price_polygon` | function |
| 56 | `get_share_price` | function |

**Read in this order:**

- Make available: `polygon`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `functools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_plan`. Re-executing this cell updates kernel state.
- Store expression result in `is_paid_polygon`. Re-executing this cell updates kernel state.
- Store expression result in `is_realtime_polygon`. Re-executing this cell updates kernel state.
- Define `is_market_open()`. Its body runs when called.
- Define `get_all_share_prices_polygon_eod()`. Its body runs when called.
- Define `get_market_for_prior_date(today)`. Its body runs when called.
- Define `get_share_price_polygon_eod(symbol)`. Its body runs when called.
- Define `get_share_price_polygon_min(symbol)`. Its body runs when called.
- Define `get_share_price_polygon(symbol)`. Its body runs when called.
- Define `get_share_price(symbol)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `lru_cache`: Reuse recent function results; expiry/freshness needs a separate policy.
- `RESTClient`: Construct the market-data client; retrieval is a separate API call.
- `market_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/mcp_params.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/mcp_params.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `researcher_mcp_server_params` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `brave_api_key`. Re-executing this cell updates kernel state.
- Store dictionary in `brave_env`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_plan`. Re-executing this cell updates kernel state.
- Store expression result in `is_paid_polygon`. Re-executing this cell updates kernel state.
- Store expression result in `is_realtime_polygon`. Re-executing this cell updates kernel state.
- Store expression result in `has_real_brave_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `trader_mcp_server_params`. Re-executing this cell updates kernel state.
- Define `researcher_mcp_server_params(name)`. Its body runs when called.
- Store list in `risk_manager_mcp_server_params`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/templates.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/templates.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.market` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `researcher_instructions` | function |
| 29 | `research_tool` | function |
| 35 | `trader_instructions` | function |
| 69 | `trade_message` | function |
| 107 | `rebalance_message` | function |
| 142 | `risk_manager_instructions` | function |
| 160 | `risk_assessment_message` | function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define `researcher_instructions()`. Its body runs when called.
- Define `research_tool()`. Its body runs when called.
- Define `trader_instructions(name)`. Its body runs when called.
- Define `trade_message(name, strategy, account)`. Its body runs when called.
- Define `rebalance_message(name, strategy, account)`. Its body runs when called.
- Define `risk_manager_instructions()`. Its body runs when called.
- Define `risk_assessment_message(all_accounts_data)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/tracers.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/tracers.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `LogTracer` | class |
| 42 | `make_trace_id` | function |
| 7 | `__init__` | function |
| 10 | `on_trace_start` | function |
| 15 | `on_trace_end` | function |
| 20 | `on_span_start` | function |
| 33 | `on_span_end` | function |
| 36 | `force_flush` | function |
| 39 | `shutdown` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LogTracer` using `TracingProcessor`. Separate the class definition, constructing an instance, and calling its methods.
- Define `make_trace_id(trader_name)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/src/utils/util.py](../../../../../../6_mcp/community_contributions/iamumarjaved/src/utils/util.py)

**Imports:**

- `enum` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `Color` | class |

**Read in this order:**

- Make available: `enum`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `css`. Re-executing this cell updates kernel state.
- Store expression result in `js`. Re-executing this cell updates kernel state.
- Define class `Color` using `Enum`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/test_news_recording.py](../../../../../../6_mcp/community_contributions/iamumarjaved/test_news_recording.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `contextlib` — Resource lifetime helpers.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.mcp_params` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `test_news_recording` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_news_recording()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/test_news_sentinel.py](../../../../../../6_mcp/community_contributions/iamumarjaved/test_news_sentinel.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `os` — Process configuration and operating-system paths.
- `src.agents.accounts` — Local or third-party dependency; follow its definition/import before execution.
- `src.agents.news_sentinel` — Local or third-party dependency; follow its definition/import before execution.
- `src.database.database` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `test_news_sentinel` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.news_sentinel`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.database.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_news_sentinel()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `news_sentinel.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/iamumarjaved/trading_floor.py](../../../../../../6_mcp/community_contributions/iamumarjaved/trading_floor.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `src.agents.news_sentinel` — Local or third-party dependency; follow its definition/import before execution.
- `src.agents.risk_manager` — Local or third-party dependency; follow its definition/import before execution.
- `src.agents.traders` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.market` — Local or third-party dependency; follow its definition/import before execution.
- `src.utils.tracers` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 35 | `create_traders` | function |
| 41 | `run_every_n_minutes` | async function |

**Read in this order:**

- Make available: `src.agents.traders`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.risk_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.agents.news_sentinel`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.utils.market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
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
- `risk_manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `news_sentinel.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `trader.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
