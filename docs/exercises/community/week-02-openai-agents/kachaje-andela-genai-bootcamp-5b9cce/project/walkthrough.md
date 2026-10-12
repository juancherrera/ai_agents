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

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

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

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [2_openai/community_contributions/kachaje-andela-genai-bootcamp/ctl.sh](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/ctl.sh): supporting source/configuration; inspect its inputs and effects before execution.
- [2_openai/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [2_openai/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/deep_research.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/deep_research.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `research_manager` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `run` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `research_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `ResearchManager(...).run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/email_agent.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/email_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `send_email` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(subject, html_body)`. Its body runs when called.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/planner_agent.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/planner_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `WebSearchItem` | class |
| 16 | `WebSearchPlan` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/research_manager.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/research_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `search_agent` — Local or third-party dependency; follow its definition/import before execution.
- `utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `writer_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `ResearchManager` | class |
| 11 | `run` | async function |
| 29 | `plan_searches` | async function |
| 39 | `perform_searches` | async function |
| 54 | `search` | async function |
| 66 | `write_report` | async function |
| 78 | `send_email` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `writer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/search_agent.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/__init__.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_email.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_email.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `tools.email` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `TestSendEmail` | class |
| 12 | `test_send_email_success` | function |
| 29 | `test_send_email_missing_api_key` | function |
| 38 | `test_send_email_failure_status_code` | function |
| 56 | `test_send_email_failure_no_body` | function |
| 74 | `test_send_email_exception` | function |
| 87 | `test_send_email_creates_correct_mail_object` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.email`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TestSendEmail`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_multiply.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_multiply.py)

**Imports:**

- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `tools.multiply` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `TestMultiplyNumbers` | class |
| 9 | `test_multiply_positive_numbers` | function |
| 14 | `test_multiply_negative_numbers` | function |
| 19 | `test_multiply_positive_and_negative` | function |
| 24 | `test_multiply_by_zero` | function |
| 29 | `test_multiply_decimals` | function |
| 34 | `test_multiply_small_decimals` | function |
| 40 | `test_multiply_calls_span` | function |
| 50 | `test_multiply_large_numbers` | function |

**Read in this order:**

- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.multiply`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TestMultiplyNumbers`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_web_search.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tests/test_web_search.py)

**Imports:**

- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `tools.web_search` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `TestWebSearch` | class |
| 11 | `test_web_search_success` | function |
| 45 | `test_web_search_no_results_retries` | function |
| 68 | `test_web_search_succeeds_on_retry` | function |
| 100 | `test_web_search_exception_handling` | function |
| 122 | `test_web_search_results_with_missing_fields` | function |
| 153 | `test_web_search_no_valid_results` | function |
| 178 | `test_web_search_calls_span` | function |

**Read in this order:**

- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.web_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TestWebSearch`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/__init__.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/__init__.py)

**Imports:**

- `.email` — Local or third-party dependency; follow its definition/import before execution.
- `.multiply` — Local or third-party dependency; follow its definition/import before execution.
- `.web_search` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.multiply`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.email`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.web_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/email.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/email.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `os` — Process configuration and operating-system paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `_html_to_plain_text` | function |
| 39 | `_send_email_impl` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_html_to_plain_text(html)`. Its body runs when called.
- Define `_send_email_impl(subject, html_body)`. Its body runs when called.
- Store expression result in `send_email`. Re-executing this cell updates kernel state.
- `function_tool`: Build a tool schema from a Python signature/docstring.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `mail.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/multiply.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/multiply.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `_multiply_numbers_impl` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_multiply_numbers_impl(a, b)`. Its body runs when called.
- Store expression result in `multiply_numbers`. Re-executing this cell updates kernel state.
- `function_tool`: Build a tool schema from a Python signature/docstring.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/web_search.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/tools/web_search.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `ddgs` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `_web_search_impl` | function |

**Read in this order:**

- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ddgs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_web_search_impl(query)`. Its body runs when called.
- Store expression result in `web_search`. Re-executing this cell updates kernel state.
- `function_tool`: Build a tool schema from a Python signature/docstring.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `formatted_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/__init__.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/globals.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/globals.py)

**Imports:**

- `.logger` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `openai` — Compatible model clients and request/response types.
- `tools.web_search` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.web_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OLLAMA_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_client`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_model`. Re-executing this cell updates kernel state.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/local_logger.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/local_logger.py)

**Imports:**

- `contextlib` — Resource lifetime helpers.
- `dataclasses` — Simple typed records.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `pathlib` — Explicit filesystem paths.
- `sqlite3` — Local database storage.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `LogEntry` | class |
| 32 | `LocalLogger` | class |
| 35 | `__init__` | function |
| 80 | `_init_database` | function |
| 102 | `_write_log` | function |
| 143 | `_create_entry` | function |
| 166 | `trace` | function |
| 227 | `span` | function |
| 286 | `log_agent_call` | function |
| 323 | `log_tool_call` | function |
| 356 | `log_span` | function |
| 377 | `get_trace` | function |
| 390 | `search_traces` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LogEntry`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LocalLogger`. Separate the class definition, constructing an instance, and calling its methods.
- `self._local.trace_stack.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `level_colors.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `params.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `all_entries.extend`: Add every item from an iterable to a list.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/logger.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/utils/logger.py)

**Imports:**

- `.local_logger` — Local or third-party dependency; follow its definition/import before execution.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `get_logger` | function |
| 14 | `trace` | function |
| 17 | `span` | function |
| 21 | `gen_trace_id` | function |

**Read in this order:**

- Make available: `.local_logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `_logger_instance`. Re-executing this cell updates kernel state.
- Define `get_logger()`. Its body runs when called.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `trace(name, trace_id)`. Its body runs when called.
- Define `span(name, message, trace_id)`. Its body runs when called.
- Define `gen_trace_id(prefix)`. Its body runs when called.
- Store list in `__all__`. Re-executing this cell updates kernel state.
- `logger.trace`: Group activity for inspection. Configured tracing may export run data.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/kachaje-andela-genai-bootcamp/writer_agent.py](../../../../../../2_openai/community_contributions/kachaje-andela-genai-bootcamp/writer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `utils.globals` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `ReportData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
