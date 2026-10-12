# url security advisor gradio sb: project walkthrough

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [2_openai/community_contributions/url_security_advisor_gradio_sb/requirements.txt](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/url_security_advisor_gradio_sb/app.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/app.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `general_manager` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `run` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `general_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `stages`. Re-executing this cell updates kernel state.
- Store expression result in `total`. Re-executing this cell updates kernel state.
- Store expression result in `gm`. Re-executing this cell updates kernel state.
- Define `run(url, email)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `gm.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/check_agents.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/check_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `instruction_check_1`. Re-executing this cell updates kernel state.
- Store expression result in `instruction_check_2`. Re-executing this cell updates kernel state.
- Store expression result in `check_agent_1`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `GEMINI_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Store expression result in `check_agent_2`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/check_manager_agent.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/check_manager_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `check_manager_instruction`. Re-executing this cell updates kernel state.
- Store expression result in `check_manager_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/check_planner_agent.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/check_planner_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `guardrail_agent` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 24 | `UrlCheckItem` | class |
| 28 | `UrlCheckPlan` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `guardrail_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_CHECKS`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `UrlCheckItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `UrlCheckPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `check_planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/email_agent.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/email_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `send_email` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(subject, html_body, email_to, email_from)`. Its body runs when called.
- Store expression result in `email_agent_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/general_manager.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/general_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.exceptions` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `check_agents` — Local or third-party dependency; follow its definition/import before execution.
- `check_manager_agent` — Local or third-party dependency; follow its definition/import before execution.
- `check_planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `report_agent` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `GeneralManager` | class |
| 13 | `run` | async function |
| 42 | `plan_checks` | async function |
| 72 | `run_checks` | async function |
| 87 | `check` | async function |
| 108 | `report` | async function |
| 116 | `send_email` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `check_planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `check_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `check_manager_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `report_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.exceptions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `GeneralManager`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/guardrail_agent.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/guardrail_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `DataCheck` | class |
| 16 | `guardrail_against_data` | async function |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `DataCheck` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `guardrail_instructions`. Re-executing this cell updates kernel state.
- Define `guardrail_against_data(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `guardrail_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/url_security_advisor_gradio_sb/report_agent.py](../../../../../../2_openai/community_contributions/url_security_advisor_gradio_sb/report_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `UrlReportData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `reporter_agent_instructions`. Re-executing this cell updates kernel state.
- Define class `UrlReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `reporter_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
