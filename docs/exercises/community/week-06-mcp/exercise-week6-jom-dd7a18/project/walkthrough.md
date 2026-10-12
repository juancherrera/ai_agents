# exercise week6 jom: project walkthrough

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [6_mcp/community_contributions/exercise_week6_jom/mcp_params.py](../../../../../../6_mcp/community_contributions/exercise_week6_jom/mcp_params.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `market` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 33 | `researcher_mcp_server_params` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store dictionary in `brave_env`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `news_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `trader_mcp_server_params`. Re-executing this cell updates kernel state.
- Define `researcher_mcp_server_params(name)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/exercise_week6_jom/sentiment_mcp.py](../../../../../../6_mcp/community_contributions/exercise_week6_jom/sentiment_mcp.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `httpx` — Sync/async HTTP requests.
- `json` — Encoding/decoding structured text.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `torch` — Local or third-party dependency; follow its definition/import before execution.
- `transformers` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 24 | `TickerInput` | class |
| 34 | `SentimentResult` | class |
| 44 | `fetch_news_newsapi` | async function |
| 86 | `init_sentiment_model` | function |
| 110 | `analyze_sentiment_huggingface` | function |
| 151 | `create_summary` | function |
| 202 | `analyze_ticker_sentiment` | async function |
| 294 | `get_sentiment_help` | async function |
| 31 | `uppercase_ticker` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `httpx`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `transformers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `torch`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Store expression result in `NEWS_API_KEY`. Re-executing this cell updates kernel state.
- Define class `TickerInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SentimentResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `fetch_news_newsapi(ticker, days_back)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `sentiment_pipeline`. Re-executing this cell updates kernel state.
- Define `init_sentiment_model()`. Its body runs when called.
- Define `analyze_sentiment_huggingface(text)`. Its body runs when called.
- Define `create_summary(ticker, articles, sentiment_scores, sentiment_labels)`. Its body runs when called.
- Define `analyze_ticker_sentiment(ticker, days_back)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_sentiment_help()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `Field`: Describe a schema field or constrain allowed values.
- `json.dumps`: Convert Python data to JSON text.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `label_map.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `article.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `unique_articles.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sentiment_scores.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sentiment_labels.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sources.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `client.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `articles.extend`: Add every item from an iterable to a list.
- `article.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/exercise_week6_jom/templates.py](../../../../../../6_mcp/community_contributions/exercise_week6_jom/templates.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `market` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `researcher_instructions` | function |
| 32 | `research_tool` | function |
| 38 | `trader_instructions` | function |
| 52 | `trade_message` | function |
| 72 | `rebalance_message` | function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define `researcher_instructions()`. Its body runs when called.
- Define `research_tool()`. Its body runs when called.
- Define `trader_instructions(name)`. Its body runs when called.
- Define `trade_message(name, strategy, account)`. Its body runs when called.
- Define `rebalance_message(name, strategy, account)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/exercise_week6_jom/traders.py](../../../../../../6_mcp/community_contributions/exercise_week6_jom/traders.py)

**Imports:**

- `accounts_client` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `aux_tool` — Local or third-party dependency; follow its definition/import before execution.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `mcp_params` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `templates` — Local or third-party dependency; follow its definition/import before execution.
- `tracers` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 41 | `get_model` | function |
| 56 | `get_researcher` | async function |
| 68 | `get_researcher_tool` | async function |
| 73 | `Trader` | class |
| 74 | `__init__` | function |
| 81 | `create_agent` | async function |
| 92 | `get_account_report` | async function |
| 98 | `run_agent` | async function |
| 109 | `run_with_mcp_servers` | async function |
| 126 | `run_with_trace` | async function |
| 132 | `run` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts_client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp_params`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `aux_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `grok_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `DEEPSEEK_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GROK_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GEMINI_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `OPENROUTER_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_TURNS`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_client`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_client`. Re-executing this cell updates kernel state.
- Store expression result in `grok_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_client`. Re-executing this cell updates kernel state.
- Define `get_model(model_name)`. Its body runs when called.
- Define `get_researcher(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_researcher_tool(mcp_servers, model_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define class `Trader`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `researcher.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json.dumps`: Convert Python data to JSON text.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
