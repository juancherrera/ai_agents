# muhammad qasim sheikh: project walkthrough

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [6_mcp/community_contributions/muhammad_qasim_sheikh/app.py](../../../../../../6_mcp/community_contributions/muhammad_qasim_sheikh/app.py)

**Imports:**

- `auditor` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `fetch_url` | function |
| 19 | `do_audit` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `auditor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DEF_HTML`. Re-executing this cell updates kernel state.
- Define `fetch_url(url)`. Its body runs when called.
- Define `do_audit(url, html, target_keywords)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/muhammad_qasim_sheikh/audit.py](../../../../../../6_mcp/community_contributions/muhammad_qasim_sheikh/audit.py)

**Imports:**

- `dataclasses` — Simple typed records.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `AuditResult` | class |
| 18 | `grade_from_scores` | function |
| 14 | `to_dict` | function |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AuditResult`. Separate the class definition, constructing an instance, and calling its methods.
- Define `grade_from_scores(score)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/muhammad_qasim_sheikh/auditor.py](../../../../../../6_mcp/community_contributions/muhammad_qasim_sheikh/auditor.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `audit` — Local or third-party dependency; follow its definition/import before execution.
- `database` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `run_audit_agent` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `audit`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DEFAULT_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define `run_audit_agent(user_query, client_session_timeout_seconds)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json_part.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json_part.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/muhammad_qasim_sheikh/database.py](../../../../../../6_mcp/community_contributions/muhammad_qasim_sheikh/database.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `pathlib` — Explicit filesystem paths.
- `sqlite3` — Local database storage.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `init_db` | function |
| 27 | `write_audit` | function |
| 50 | `list_audits` | function |
| 64 | `read_audit` | function |

**Read in this order:**

- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DB_PATH`. Re-executing this cell updates kernel state.
- Define `init_db()`. Its body runs when called.
- Define `write_audit(title, content, overall_score, report)`. Its body runs when called.
- Define `list_audits(limit)`. Its body runs when called.
- Define `read_audit(audit_id)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `conn.close`: Release the resource; match setup with cleanup.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/muhammad_qasim_sheikh/seo_audit_server.py](../../../../../../6_mcp/community_contributions/muhammad_qasim_sheikh/seo_audit_server.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `math` — Local or third-party dependency; follow its definition/import before execution.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `AuditArgs` | class |
| 17 | `AuditReport` | class |
| 25 | `_word_count` | function |
| 29 | `_grade` | function |
| 41 | `compute_readability` | function |
| 56 | `compute_seo` | function |
| 77 | `compute_tone` | function |
| 90 | `compute_plagiarism` | function |
| 98 | `full_audit` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `math`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define class `AuditArgs` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `AuditReport` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `_word_count(text)`. Its body runs when called.
- Define `_grade(score)`. Its body runs when called.
- Define `compute_readability(args)`. Its body runs when called.
- Define `compute_seo(args)`. Its body runs when called.
- Define `compute_tone(args)`. Its body runs when called.
- Define `compute_plagiarism(args)`. Its body runs when called.
- Define `full_audit(args)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `Field`: Describe a schema field or constrain allowed values.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
