# deep research with clarifier muthama: project walkthrough

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

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Module-by-module reading map

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/deep_research.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/deep_research.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `research_manager` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `get_clarifying_questions_ui` | async function |
| 18 | `submit_answer` | async function |
| 35 | `run` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `research_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_clarifying_questions_ui(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `submit_answer(answer, answers_list, questions_list, current_index, original_query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `run(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `answers_list.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `ResearchManager(...).run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/clarifier_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/clarifier_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `logging` — Application diagnostics.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 42 | `CreateQuestions` | class |
| 58 | `ClarifyingQuestions` | class |
| 80 | `validate_model_output` | function |
| 51 | `ensure_open_ended` | function |
| 64 | `validate_questions` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `OneLine`. Re-executing this cell updates kernel state.
- Store expression result in `ClarifyingQuestionText`. Re-executing this cell updates kernel state.
- Define class `CreateQuestions` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ClarifyingQuestions` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `validate_model_output(raw)`. Its body runs when called.
- Store expression result in `clarifier_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/contextualizer_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/contextualizer_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `logging` — Application diagnostics.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 50 | `ClarifyingQA` | class |
| 66 | `ContextualizerInput` | class |
| 83 | `ContextualizedQuery` | class |
| 120 | `_split_sentences` | function |
| 134 | `validate_model_output` | function |
| 59 | `normalize_empty_answer` | function |
| 76 | `ensure_consistency` | function |
| 93 | `clean_and_validate` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `ShortText`. Re-executing this cell updates kernel state.
- Define class `ClarifyingQA` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ContextualizerInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ContextualizedQuery` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `_split_sentences(text)`. Its body runs when called.
- Define `validate_model_output(raw)`. Its body runs when called.
- Store expression result in `contextualizing_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/email_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/email_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `os` — Process configuration and operating-system paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `tenacity` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 30 | `EmailResult` | class |
| 37 | `EmailError` | class |
| 41 | `ConfigurationError` | class |
| 45 | `ValidationError` | class |
| 49 | `SendGridError` | class |
| 53 | `_sanitize_html` | function |
| 67 | `_get_sendgrid_client` | function |
| 76 | `_validate_addresses` | function |
| 94 | `_send_via_sendgrid` | function |
| 145 | `send_email` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tenacity`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `SENDGRID_API_KEY_ENV`. Re-executing this cell updates kernel state.
- Store expression result in `DEFAULT_FROM_ENV`. Re-executing this cell updates kernel state.
- Store expression result in `DEFAULT_TO_ENV`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_SUBJECT_LENGTH`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_BODY_BYTES`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `EmailResult` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EmailError` using `Exception`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ConfigurationError` using `EmailError`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ValidationError` using `EmailError`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SendGridError` using `EmailError`. Separate the class definition, constructing an instance, and calling its methods.
- Define `_sanitize_html(html)`. Its body runs when called.
- Define `_get_sendgrid_client()`. Its body runs when called.
- Define `_validate_addresses(addr)`. Its body runs when called.
- Define `_send_via_sendgrid(subject, html_body, from_email, to_emails)`. Its body runs when called.
- Define `send_email(subject, html_body, from_email, to_email)`. Its body runs when called.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `client.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `mail.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `send_result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/planner_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/planner_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `logging` — Application diagnostics.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `WebSearchItem` | class |
| 36 | `WebSearchPlan` | class |
| 49 | `validate_model_output` | function |
| 27 | `validate_query` | function |
| 40 | `enforce_search_count_and_uniqueness` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `ShortText`. Re-executing this cell updates kernel state.
- Store expression result in `QueryText`. Re-executing this cell updates kernel state.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `validate_model_output(raw)`. Its body runs when called.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/search_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/search_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `SearchResult` | class |
| 29 | `validate_paragraphs_and_length` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `SummaryText`. Re-executing this cell updates kernel state.
- Define class `SearchResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/writer_agent.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/my_agents/writer_agent.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `logging` — Application diagnostics.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 27 | `_count_words` | function |
| 31 | `_count_sentences` | function |
| 35 | `ReportData` | class |
| 108 | `validate_model_output` | function |
| 51 | `validate_summary_sentences` | function |
| 62 | `validate_markdown_and_length` | function |
| 77 | `validate_followups` | function |
| 96 | `cross_check_summary_vs_report` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `ShortLine`. Re-executing this cell updates kernel state.
- Store expression result in `MarkdownText`. Re-executing this cell updates kernel state.
- Store expression result in `FollowUpQuestion`. Re-executing this cell updates kernel state.
- Define `_count_words(text)`. Its body runs when called.
- Define `_count_sentences(text)`. Its body runs when called.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `validate_model_output(raw)`. Its body runs when called.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.
- `cleaned.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep_research_with_clarifier_muthama/research_manager.py](../../../../../../2_openai/community_contributions/deep_research_with_clarifier_muthama/research_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `my_agents.clarifier_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.contextualizer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.search_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.writer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `ResearchManager` | class |
| 14 | `run` | async function |
| 42 | `get_clarifying_questions` | async function |
| 59 | `run_contextualizer` | async function |
| 75 | `plan_searches` | async function |
| 85 | `perform_searches` | async function |
| 100 | `search` | async function |
| 112 | `write_report` | async function |
| 124 | `send_email` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.clarifier_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.contextualizer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.writer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
