# deep research agents: project walkthrough

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

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

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

## Configuration and supporting files

- [2_openai/community_contributions/deep-research-agents/README.md](../../../../../../2_openai/community_contributions/deep-research-agents/README.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/deep-research-agents/ai_agents/manager_agent.py](../../../../../../2_openai/community_contributions/deep-research-agents/ai_agents/manager_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `tools.email_writer` — Local or third-party dependency; follow its definition/import before execution.
- `tools.question_generator` — Local or third-party dependency; follow its definition/import before execution.
- `tools.report_writer` — Local or third-party dependency; follow its definition/import before execution.
- `tools.search_executor` — Local or third-party dependency; follow its definition/import before execution.
- `tools.search_terms_generator` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `ManagerAgent` | class |
| 44 | `__init__` | function |
| 63 | `reset` | function |
| 66 | `run` | async function |

**Read in this order:**

- Make available: `tools.email_writer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.question_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.report_writer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.search_terms_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.search_executor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_QUESTIONS`. Re-executing this cell updates kernel state.
- Store formatted string in `MANAGER_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ManagerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `self.context.qa_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.reset`: Reset state. For accounts, this can replace stored data.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/ai_agents/search_agent.py](../../../../../../2_openai/community_contributions/deep-research-agents/ai_agents/search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `schema` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 35 | `run_search` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SEARCH_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- Define `run_search(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/main.py](../../../../../../2_openai/community_contributions/deep-research-agents/main.py)

**Imports:**

- `ai_agents.manager_agent` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `utils.loading_message` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `agent_chat` | async function |
| 39 | `main` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ai_agents.manager_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.loading_message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `manager`. Re-executing this cell updates kernel state.
- Define `agent_chat(user_message, chat_history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `temp_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `chat_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/schema.py](../../../../../../2_openai/community_contributions/deep-research-agents/schema.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `Question` | class |
| 16 | `Answer` | class |
| 21 | `QAItem` | class |
| 26 | `ResearchContext` | class |
| 61 | `WebSearchItem` | class |
| 70 | `WebSearchPlan` | class |
| 81 | `SearchResult` | class |
| 86 | `ExecutedSearchPlan` | class |
| 90 | `ReportData` | class |
| 96 | `EmailStatus` | class |
| 30 | `to_json_str` | function |
| 34 | `to_system_content` | function |
| 37 | `to_transcript` | function |
| 54 | `to_input_data` | function |
| 65 | `to_json_str` | function |
| 74 | `ensure_no_duplicates` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `RoleAgent`. Re-executing this cell updates kernel state.
- Store expression result in `RoleUser`. Re-executing this cell updates kernel state.
- Define class `Question` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Answer` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `QAItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ResearchContext` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SearchResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ExecutedSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EmailStatus` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `json.dumps`: Convert Python data to JSON text.
- `transcript.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/email_writer.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/email_writer.py)

**Imports:**

- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `tools.send_email` — Local or third-party dependency; follow its definition/import before execution.
- `tools.tool_wrapper` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `tools.tool_wrapper`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.send_email`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `EMAIL_WRITER_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `email_writer_tool`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/question_generator.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/question_generator.py)

**Imports:**

- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `tools.tool_wrapper` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.tool_wrapper`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `QUESTION_GENERATOR_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `question_generator_tool`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/report_writer.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/report_writer.py)

**Imports:**

- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `tools.tool_wrapper` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `tools.tool_wrapper`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `REPORT_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `report_writer_tool`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/search_executor.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/search_executor.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `ai_agents.search_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `schema` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `execute_search_plan` | async function |

**Read in this order:**

- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ai_agents.search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `execute_search_plan(plan)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/search_terms_generator.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/search_terms_generator.py)

**Imports:**

- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `tools.tool_wrapper` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.tool_wrapper`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `SEARCH_TERMS_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_terms_generator_tool`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/send_email.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/send_email.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `os` — Process configuration and operating-system paths.
- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `send_email` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(subject, html_body)`. Its body runs when called.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/tools/tool_wrapper.py](../../../../../../2_openai/community_contributions/deep-research-agents/tools/tool_wrapper.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.tool` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `schema` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `tool_from_agent` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `tool_from_agent(agent_name, agent_instructions, output_type, tool_name, tool_description)`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research-agents/utils/loading_message.py](../../../../../../2_openai/community_contributions/deep-research-agents/utils/loading_message.py)

**Read in this order:**

- Store expression result in `loading_html`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
