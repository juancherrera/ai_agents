# customer care telegram: project walkthrough

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

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

## Configuration and supporting files

- [2_openai/community_contributions/customer_care_telegram/README.md](../../../../../../2_openai/community_contributions/customer_care_telegram/README.md): 🤖 Telegram Customer Care Bot. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [2_openai/community_contributions/customer_care_telegram/requirements.txt](../../../../../../2_openai/community_contributions/customer_care_telegram/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/customer_care_telegram/modules/__init__.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/agents/__init__.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/agents/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/agents/order_status_agent.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/agents/order_status_agent.py)

**Imports:**

- `google.adk.agents` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.check_order_status` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `create_order_status_agent` | function |

**Read in this order:**

- Make available: `google.adk.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.check_order_status`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `create_order_status_agent()`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/agents/root_agent.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/agents/root_agent.py)

**Imports:**

- `google.adk.agents` — Local or third-party dependency; follow its definition/import before execution.
- `google.adk.agents.sequential_agent` — Local or third-party dependency; follow its definition/import before execution.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `create_root_agent` | function |

**Read in this order:**

- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.agents.sequential_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `create_root_agent(config, product_query_agent, order_form_agent, convincing_response_agent, order_status_agent)`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/agents/sequential_agents.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/agents/sequential_agents.py)

**Imports:**

- `google.adk.agents` — Local or third-party dependency; follow its definition/import before execution.
- `google.adk.agents.sequential_agent` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.calculate_order_price` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.run_pandas_query_tool` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.save_order` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `create_agents` | function |

**Read in this order:**

- Make available: `google.adk.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.agents.sequential_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.run_pandas_query_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.save_order`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.calculate_order_price`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `create_agents(df, client)`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/config.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/config.py)

**Imports:**

- `dataclasses` — Simple typed records.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `Config` | class |
| 16 | `validate` | function |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Config`. Separate the class definition, constructing an instance, and calling its methods.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/in_memory_session.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/in_memory_session.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `google.adk.sessions` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `InMemorySessionService` | class |
| 7 | `__init__` | function |
| 10 | `create_session` | async function |
| 31 | `get_session` | async function |
| 38 | `get_custom_data` | async function |
| 43 | `delete_session` | async function |
| 48 | `list_sessions` | async function |
| 55 | `list_events` | async function |
| 58 | `update_session` | async function |
| 65 | `append_history` | async function |

**Read in this order:**

- Make available: `google.adk.sessions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `InMemorySessionService` using `BaseSessionService`. Separate the class definition, constructing an instance, and calling its methods.
- `self._sessions.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/setup_logging.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/setup_logging.py)

**Imports:**

- `logging` — Application diagnostics.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `setup_logging` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `setup_logging()`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/calculate_order_price.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/calculate_order_price.py)

**Imports:**

- `fuzzywuzzy` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.setup_sheets` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `calculate_order_price` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.setup_sheets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fuzzywuzzy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `_`, `df`. Re-executing this cell updates kernel state.
- Define `calculate_order_price(product_name, quantity)`. Its body runs when called.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/check_order_status.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/check_order_status.py)

**Imports:**

- `fuzzywuzzy` — Local or third-party dependency; follow its definition/import before execution.
- `gspread` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.setup_sheets` — Local or third-party dependency; follow its definition/import before execution.
- `pandas` — Tabular data.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `check_order_status` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fuzzywuzzy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gspread`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.setup_sheets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `client`, `df`. Re-executing this cell updates kernel state.
- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `check_order_status(order_id, customer_name, phone_number)`. Its body runs when called.
- `json.dumps`: Convert Python data to JSON text.
- `client.open`: Open a file or resource. Inspect the mode and path before running it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/run_pandas_query_tool.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/run_pandas_query_tool.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.setup_sheets` — Local or third-party dependency; follow its definition/import before execution.
- `pandas` — Tabular data.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `run_query_from_agent` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.setup_sheets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `_`, `df`. Re-executing this cell updates kernel state.
- Define `run_query_from_agent(query_str, use_head)`. Its body runs when called.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/save_order.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/save_order.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `gspread` — Local or third-party dependency; follow its definition/import before execution.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.send_email` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.setup_sheets` — Local or third-party dependency; follow its definition/import before execution.
- `random` — Demo/simulation randomness.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `save_order_to_sheet` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Make available: `modules.tools.setup_sheets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `client`, `df`. Re-executing this cell updates kernel state.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gspread`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.send_email`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `save_order_to_sheet(order_details, order_id)`. Its body runs when called.
- `client.open`: Open a file or resource. Inspect the mode and path before running it.
- `order_details.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/send_email.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/send_email.py)

**Imports:**

- `email.mime.text` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `smtplib` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `send_email` | function |

**Read in this order:**

- Make available: `smtplib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email.mime.text`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(recipient, body)`. Its body runs when called.
- `server.send_message`: Deliver a message to an agent identity through a runtime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/modules/tools/setup_sheets.py](../../../../../../2_openai/community_contributions/customer_care_telegram/modules/tools/setup_sheets.py)

**Imports:**

- `google.oauth2.service_account` — Local or third-party dependency; follow its definition/import before execution.
- `gspread` — Local or third-party dependency; follow its definition/import before execution.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `pandas` — Tabular data.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `initialize_google_sheets` | function |

**Read in this order:**

- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gspread`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.oauth2.service_account`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `initialize_google_sheets(config)`. Its body runs when called.
- `client.open`: Open a file or resource. Inspect the mode and path before running it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_telegram/telegram_customer_agent.py](../../../../../../2_openai/community_contributions/customer_care_telegram/telegram_customer_agent.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `google.adk.agents` — Local or third-party dependency; follow its definition/import before execution.
- `google.adk.runners` — Local or third-party dependency; follow its definition/import before execution.
- `google.genai` — Local or third-party dependency; follow its definition/import before execution.
- `logging` — Application diagnostics.
- `modules.agents.order_status_agent` — Local or third-party dependency; follow its definition/import before execution.
- `modules.agents.root_agent` — Local or third-party dependency; follow its definition/import before execution.
- `modules.agents.sequential_agents` — Local or third-party dependency; follow its definition/import before execution.
- `modules.config` — Local or third-party dependency; follow its definition/import before execution.
- `modules.in_memory_session` — Local or third-party dependency; follow its definition/import before execution.
- `modules.setup_logging` — Local or third-party dependency; follow its definition/import before execution.
- `modules.tools.setup_sheets` — Local or third-party dependency; follow its definition/import before execution.
- `quart` — Local or third-party dependency; follow its definition/import before execution.
- `telegram` — Local or third-party dependency; follow its definition/import before execution.
- `telegram.ext` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 24 | `SessionServiceSingleton` | class |
| 35 | `setup_session_and_runner` | function |
| 45 | `call_agent_async` | async function |
| 168 | `start_handler` | async function |
| 177 | `agent_handler` | async function |
| 198 | `error_handler` | async function |
| 204 | `set_webhook` | async function |
| 222 | `create_quart_app` | function |
| 281 | `create_telegram_application` | async function |
| 290 | `main` | async function |
| 28 | `get_instance` | function |
| 226 | `index` | async function |
| 230 | `receive_telegram_update` | async function |
| 246 | `check_session` | async function |
| 266 | `list_sessions` | async function |
| 93 | `run_generator` | function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `quart`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `telegram`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `telegram.ext`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.runners`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.genai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.setup_logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.tools.setup_sheets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.agents.sequential_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.in_memory_session`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.agents.order_status_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.agents.root_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `SessionServiceSingleton`. Separate the class definition, constructing an instance, and calling its methods.
- Define `setup_session_and_runner(config, agent)`. Its body runs when called.
- Define `call_agent_async(user_id, session_id, query, runner, session_service, logger)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `start_handler(update, context)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `agent_handler(update, context, config, session_service, runner, logger)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `error_handler(update, context)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `set_webhook(application, config, logger)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `create_quart_app(config, telegram_app, session_service)`. Its body runs when called.
- Define `create_telegram_application(config)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `custom_data.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `custom_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `content_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `telegram_app.start`: Start the receiver runtime/server/resource.
- `runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `telegram_app.stop`: Stop the receiver runtime/server/resource.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
