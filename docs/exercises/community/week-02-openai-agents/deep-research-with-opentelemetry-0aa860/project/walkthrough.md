# deep research with opentelemetry: project walkthrough

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

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [2_openai/community_contributions/deep_research_with_opentelemetry/docker-compose.yaml](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/docker-compose.yaml): configuration sections `networks`, `services`. Match these names with code references and supplied input placeholders.
- [2_openai/community_contributions/deep_research_with_opentelemetry/otel-collector-config.yaml](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/otel-collector-config.yaml): configuration sections `receivers`, `processors`, `exporters`, `service`. Match these names with code references and supplied input placeholders.
- [2_openai/community_contributions/deep_research_with_opentelemetry/pyproject.toml](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [2_openai/community_contributions/deep_research_with_opentelemetry/README.md](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/README.md): OpenAI Agents SDK Deep Research Example with OpenTelemetry. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/deep_research_with_opentelemetry/deep_research.py](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/deep_research.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `openlit` — Local or third-party dependency; follow its definition/import before execution.
- `opentelemetry` — Local or third-party dependency; follow its definition/import before execution.
- `opentelemetry._logs` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `search_agent` — Local or third-party dependency; follow its definition/import before execution.
- `writer_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 26 | `DeepResearchManager` | class |
| 75 | `main` | function |
| 28 | `run` | async function |
| 39 | `plan_searches` | async function |
| 46 | `search` | async function |
| 52 | `perform_searches` | async function |
| 60 | `write_report` | async function |
| 68 | `send_email` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `writer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `opentelemetry`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `opentelemetry`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `opentelemetry._logs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openlit`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `tracer`. Re-executing this cell updates kernel state.
- Store expression result in `meter_provider`. Re-executing this cell updates kernel state.
- Store expression result in `meter`. Re-executing this cell updates kernel state.
- Store expression result in `logger_provider`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define class `DeepResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `deep_research_manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_opentelemetry/email_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/email_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `send_email` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(subject, html_body)`. Its body runs when called.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_opentelemetry/planner_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/planner_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `WebSearchItem` | class |
| 16 | `WebSearchPlan` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_opentelemetry/search_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.model_settings` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.model_settings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_opentelemetry/writer_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_opentelemetry/writer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `ReportData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
