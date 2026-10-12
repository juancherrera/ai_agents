# ranskills dynamic db queries: project walkthrough

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

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [6_mcp/community_contributions/ranskills_dynamic_db_queries/README.md](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/README.md): MCP Server Project. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [6_mcp/community_contributions/ranskills_dynamic_db_queries/requirements.txt](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/app.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/app.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `common` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `langchain.agents` — Model/tool wrappers.
- `langchain.chat_models` — Model/tool wrappers.
- `langchain_mcp_adapters.client` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_openai` — Compatible model integration for LangChain.
- `logging` — Application diagnostics.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `rich.console` — Formatted console output.
- `rich.markdown` — Formatted console output.
- `typer` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 65 | `run` | function |
| 69 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_mcp_adapters.client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.console`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.markdown`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.chat_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `common`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store dictionary in `LLM_PROVIDERS`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_PROVIDER`. Re-executing this cell updates kernel state.
- Store expression result in `api_key`, `base_url`, `model`. Re-executing this cell updates kernel state.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `server_path`. Re-executing this cell updates kernel state.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `console`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `console.print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `agent.ainvoke`: Run an asynchronous graph or model operation with await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/common/__init__.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/common/__init__.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.logger` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/common/config.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/common/config.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `base_dir`. Re-executing this cell updates kernel state.
- Store expression result in `data_dir`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/common/logger.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/common/logger.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `logging` — Application diagnostics.

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/__init__.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/__init__.py)

**Read in this order:**

- Store expression result in `__version__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/config.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/config.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `base_dir`. Re-executing this cell updates kernel state.
- Store expression result in `data_dir`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/db.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/db.py)

**Imports:**

- `config` — Local or third-party dependency; follow its definition/import before execution.
- `sqlalchemy` — Local or third-party dependency; follow its definition/import before execution.
- `sqlalchemy.ext.asyncio` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `DatabaseInspector` | class |
| 14 | `__init__` | function |
| 21 | `initialize` | async function |
| 31 | `close` | async function |
| 36 | `get_all_tables` | async function |
| 43 | `get_table_schema` | async function |
| 61 | `get_complete_schema` | async function |
| 72 | `execute_query` | async function |
| 102 | `validate_query` | async function |
| 48 | `sync_inspect` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlalchemy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlalchemy.ext.asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `DatabaseInspector`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `DATABASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `db_file`. Re-executing this cell updates kernel state.
- Store formatted string in `DATABASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `db_inspector`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.engine.connect`: Open a database or network/server connection; identify the receiver.
- `conn.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/prompts.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/prompts.py)

**Imports:**

- `db` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `get_prompts` | function |
| 59 | `get_prompt` | async function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `db`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_prompts()`. Its body runs when called.
- Define `get_prompt(name, arguments)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `arguments.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/resources.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/resources.py)

**Imports:**

- `db` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `get_resource` | async function |
| 35 | `list_resources` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `db`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_resource(uri)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `list_resources()`. Its body runs when called.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/server.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/server.py)

**Imports:**

- `db` — Local or third-party dependency; follow its definition/import before execution.
- `logging` — Application diagnostics.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `prompts` — Local or third-party dependency; follow its definition/import before execution.
- `resources` — Local or third-party dependency; follow its definition/import before execution.
- `tools` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `query_database` | async function |
| 31 | `describe_table` | async function |
| 37 | `list_tables` | async function |
| 43 | `validate_sql` | async function |
| 50 | `complete_schema` | async function |
| 56 | `table_list` | async function |
| 62 | `table_schema` | async function |
| 69 | `analyze_table` | async function |
| 75 | `generate_query` | async function |
| 83 | `find_relationships` | async function |
| 88 | `main` | async function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resources`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `db`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `query_database(sql, limit)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `describe_table(table_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `list_tables()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `validate_sql(sql)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `complete_schema()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `table_list()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `table_schema(table_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `analyze_table(table_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `generate_query(description, tables)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `find_relationships(table1, table2)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `db_inspector.close`: Release the resource; match setup with cleanup.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/tools.py](../../../../../../6_mcp/community_contributions/ranskills_dynamic_db_queries/mcp_server/tools.py)

**Imports:**

- `db` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `query_database` | async function |
| 25 | `describe_table` | async function |
| 42 | `list_tables` | async function |
| 56 | `validate_sql` | async function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `db`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `query_database(sql, limit)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `describe_table(table_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `list_tables()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `validate_sql(sql)`. Its body runs when called; await/schedule the coroutine to execute async work.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
