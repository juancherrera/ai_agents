# sach9 deep research ollama: project walkthrough

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

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [2_openai/community_contributions/sach9-deep-research-ollama/base_model.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/base_model.py)

**Imports:**

- `agents.extensions.models.litellm_model` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

**Read in this order:**

- Make available: `agents.extensions.models.litellm_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ollama_model`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_model_eval`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/deep_research.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/deep_research.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `research_manager` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `run` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `research_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `ResearchManager(...).run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/eval_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/eval_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `EvalData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `EvalData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `eval_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/main_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/main_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `writer_agent` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `writer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `main_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/msg_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/msg_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.
- `socket` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `send_msg` | function |
| 24 | `push` | function |

**Read in this order:**

- Make available: `os`, `socket`, `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `telegram_bot_token`. Re-executing this cell updates kernel state.
- Store expression result in `telegram_chat_id`. Re-executing this cell updates kernel state.
- Store formatted string in `telegram_url`. Re-executing this cell updates kernel state.
- Store expression result in `telegram_sync`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Define `send_msg(query, concise_report)`. Its body runs when called.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `msg_agent`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/planner_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/planner_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `WebSearchItem` | class |
| 16 | `WebSearchPlan` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/research_manager.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/research_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `eval_agent` — Local or third-party dependency; follow its definition/import before execution.
- `main_agent` — Local or third-party dependency; follow its definition/import before execution.
- `msg_agent` — Local or third-party dependency; follow its definition/import before execution.
- `planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `search_agent` — Local or third-party dependency; follow its definition/import before execution.
- `writer_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `ResearchManager` | class |
| 15 | `run` | async function |
| 36 | `main_report` | async function |
| 45 | `eval_report` | async function |
| 55 | `plan_searches` | async function |
| 65 | `perform_searches` | async function |
| 80 | `search` | async function |
| 93 | `write_report` | async function |
| 105 | `send_msg` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `main_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eval_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `writer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `msg_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/search_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `ddgs` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `web_search` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ddgs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define `web_search(query, safe_search)`. Its body runs when called.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `r.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sach9-deep-research-ollama/writer_agent.py](../../../../../../2_openai/community_contributions/sach9-deep-research-ollama/writer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `base_model` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `ReportData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
