# deep research clarifying evaluator agentic pattern: project walkthrough

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

## Module-by-module reading map

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/clarification_agent.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/clarification_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Store expression result in `CLARIFYING_QUESTIONS`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `clarification_agent`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/constants.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/constants.py)

**Read in this order:**

- Store expression result in `default_model`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_default_model`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_default_model`. Re-executing this cell updates kernel state.
- Store expression result in `groq_default_model`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/deep_research.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/deep_research.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `cProfile` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `openai.types.responses` — Compatible model clients and request/response types.
- `research_manager` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `DeepResearchHooks` | class |
| 51 | `deep_research` | async function |
| 16 | `on_agent_start` | async function |
| 20 | `on_tool_start` | async function |
| 28 | `on_tool_end` | async function |
| 36 | `on_handoff` | async function |
| 42 | `on_agent_end` | async function |
| 116 | `render_question_inputs` | function |

**Read in this order:**

- Make available: `cProfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai.types.responses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `research_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `DeepResearchHooks` using `RunHooks`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `hooks`. Re-executing this cell updates kernel state.
- Define `deep_research()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `args.remove`: Delete an item/file depending on the receiver. Check the target.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `chat_interface.launch`: Start the UI/server; defining the UI alone does not start it.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `Runner.run_streamed`: Begin a streamed agent run; select the relevant event types from its stream.
- `dynamic_inputs.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/email_manager.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/email_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `send_html_email` | async function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `email`. Re-executing this cell updates kernel state.
- Define `send_html_email(subject, html_body)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store formatted string in `subject_writer_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer_agent`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer_tool`. Re-executing this cell updates kernel state.
- Store formatted string in `email_formatter_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `email_formatter_agent`. Re-executing this cell updates kernel state.
- Store expression result in `email_formatter_tool`. Re-executing this cell updates kernel state.
- Store formatted string in `email_manager_instructions`. Re-executing this cell updates kernel state.
- Store list in `email_manager_tools`. Re-executing this cell updates kernel state.
- Store expression result in `email_manager`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `subject_writer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `email_formatter_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/evaluator.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/evaluator.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `EvaluationModel` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Define class `EvaluationModel` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `evaluator_agent`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/guardrail_agent.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/guardrail_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `AbuseCheckOutput` | class |
| 22 | `guardrail_against_abuse` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AbuseCheckOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `guardrail_agent`. Re-executing this cell updates kernel state.
- Define `guardrail_against_abuse(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/models.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/models.py)

**Imports:**

- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `ClarifyingQuestions` | class |
| 8 | `ResearchManagerUpdates` | class |
| 16 | `EvaluationModel` | class |
| 21 | `Clarification` | class |
| 24 | `WebSearchTerm` | class |
| 29 | `WebSearchPlan` | class |
| 32 | `ReportData` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ClarifyingQuestions` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ResearchManagerUpdates` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluationModel` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Clarification` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchTerm` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/planner_agent.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/planner_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `ClarifyingQuestions` | class |
| 16 | `WebSearchTerm` | class |
| 21 | `WebSearchPlan` | class |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ClarifyingQuestions` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchTerm` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/reporter_agent.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/reporter_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `ReportData` | class |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `reporter_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/research_manager.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/research_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `clarification_agent` — Local or third-party dependency; follow its definition/import before execution.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `email_manager` — Local or third-party dependency; follow its definition/import before execution.
- `evaluator` — Local or third-party dependency; follow its definition/import before execution.
- `guardrail_agent` — Local or third-party dependency; follow its definition/import before execution.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `planner_agent` — Local or third-party dependency; follow its definition/import before execution.
- `reporter_agent` — Local or third-party dependency; follow its definition/import before execution.
- `search_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 52 | `logger_tool` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `planner_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `search_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reporter_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `evaluator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `clarification_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `guardrail_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Store expression result in `CLARIFYING_QUESTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_RETRY`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `clarification_agent_tool`. Re-executing this cell updates kernel state.
- Store expression result in `planner_agent_tool`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent_tool`. Re-executing this cell updates kernel state.
- Store expression result in `reporter_agent_tool`. Re-executing this cell updates kernel state.
- Store expression result in `evaluator_agent_tool`. Re-executing this cell updates kernel state.
- Define `logger_tool(msg)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Store expression result in `research_manager`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `clarification_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `planner_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `search_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `reporter_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `evaluator_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/search_agent.py](../../../../../../2_openai/community_contributions/deep-research_clarifying_evaluator_agentic_pattern/search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
