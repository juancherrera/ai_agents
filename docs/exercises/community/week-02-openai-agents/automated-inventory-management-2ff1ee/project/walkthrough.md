# Automated Inventory Management: project walkthrough

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

## Configuration and supporting files

- [2_openai/community_contributions/Automated_Inventory_Management/pyproject.toml](../../../../../../2_openai/community_contributions/Automated_Inventory_Management/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [2_openai/community_contributions/Automated_Inventory_Management/README.md](../../../../../../2_openai/community_contributions/Automated_Inventory_Management/README.md): Agentic AI Inventory Management System. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/Automated_Inventory_Management/ai_agents/database_manager_agent.py](../../../../../../2_openai/community_contributions/Automated_Inventory_Management/ai_agents/database_manager_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `sqlite3` — Local database storage.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `create_database` | function |
| 28 | `create_products_table` | function |
| 49 | `add_test_products_table_data` | function |
| 65 | `delete_all_rows_products_table` | function |
| 78 | `sell_products` | function |
| 105 | `DatabaseManagerAgent` | class |
| 106 | `__init__` | function |
| 124 | `run_task` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `create_database()`. Its body runs when called.
- Define `create_products_table(db_path)`. Its body runs when called.
- Define `add_test_products_table_data(db_path)`. Its body runs when called.
- Define `delete_all_rows_products_table(db_path)`. Its body runs when called.
- Define `sell_products(product, quantity, db_path)`. Its body runs when called.
- Define class `DatabaseManagerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `conn.close`: Release the resource; match setup with cleanup.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Automated_Inventory_Management/ai_agents/product_sales_agent.py](../../../../../../2_openai/community_contributions/Automated_Inventory_Management/ai_agents/product_sales_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `ai_agents.database_manager_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `sell_products` | async function |
| 17 | `ProductSalesAgent` | class |
| 18 | `__init__` | function |
| 31 | `run_task` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ai_agents.database_manager_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `sell_products(product, quantity)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define class `ProductSalesAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Automated_Inventory_Management/main.py](../../../../../../2_openai/community_contributions/Automated_Inventory_Management/main.py)

**Imports:**

- `ai_agents.database_manager_agent` — Local or third-party dependency; follow its definition/import before execution.
- `ai_agents.product_sales_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ai_agents.database_manager_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ai_agents.product_sales_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
