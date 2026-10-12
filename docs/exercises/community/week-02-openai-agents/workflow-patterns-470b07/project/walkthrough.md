# workflow patterns: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/README.md](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/README.md): How to Get the Google Search API Keys (Step-by-Step). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/build_agents.py](../../../../../../2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/build_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `llm_models` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `llm_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `model_provider`. Re-executing this cell updates kernel state.
- Store expression result in `available_model`. Re-executing this cell updates kernel state.
- Store expression result in `curriculum_agent`. Re-executing this cell updates kernel state.
- Store expression result in `curriculum_checker_agent`. Re-executing this cell updates kernel state.
- Store expression result in `lesson_writer_agent`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/configs.py](../../../../../../2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/configs.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `LLMConfig` | class |
| 52 | `CurriculumCheckOutput` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLMConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `AVAILABLE_LLMS`. Re-executing this cell updates kernel state.
- Store expression result in `LLMProvider`. Re-executing this cell updates kernel state.
- Define class `CurriculumCheckOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/llm_models.py](../../../../../../2_openai/community_contributions/workflow_patterns/1-Prompt_Chaining/llm_models.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | class |
| 24 | `LLMManager` | class |
| 19 | `__init__` | function |
| 32 | `__init__` | function |
| 36 | `_create_llm_instance` | function |
| 74 | `get_llm` | function |
| 92 | `get_model` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLM`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LLMManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm_manager`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._instances.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/2-Parallelization/build_agents.py](../../../../../../2_openai/community_contributions/workflow_patterns/2-Parallelization/build_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `llm_models` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `llm_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Store expression result in `groq_model`. Re-executing this cell updates kernel state.
- Store expression result in `ag_coding_explainer`. Re-executing this cell updates kernel state.
- Store expression result in `ag_explanation_picker`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/2-Parallelization/configs.py](../../../../../../2_openai/community_contributions/workflow_patterns/2-Parallelization/configs.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `LLMConfig` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLMConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `AVAILABLE_LLMS`. Re-executing this cell updates kernel state.
- Store expression result in `LLMProvider`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/2-Parallelization/llm_models.py](../../../../../../2_openai/community_contributions/workflow_patterns/2-Parallelization/llm_models.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | class |
| 24 | `LLMManager` | class |
| 19 | `__init__` | function |
| 32 | `__init__` | function |
| 36 | `_create_llm_instance` | function |
| 74 | `get_llm` | function |
| 92 | `get_model` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLM`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LLMManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm_manager`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._instances.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/basic_routing.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/basic_routing.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `build_agents` — Local or third-party dependency; follow its definition/import before execution.
- `rich.console` — Formatted console output.
- `rich.markdown` — Formatted console output.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `build_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.console`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.markdown`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `console`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `console.print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/build_agents.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/build_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `llm_models` — Local or third-party dependency; follow its definition/import before execution.
- `schemas` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 50 | `check_if_on_topic` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `llm_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `available_model`. Re-executing this cell updates kernel state.
- Store expression result in `ag_python_tutor`. Re-executing this cell updates kernel state.
- Store expression result in `ag_js_tutor`. Re-executing this cell updates kernel state.
- Store expression result in `ag_sql_tutor`. Re-executing this cell updates kernel state.
- Store expression result in `guardrail_topic_checker_agent`. Re-executing this cell updates kernel state.
- Define `check_if_on_topic(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `routing_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/configs.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/configs.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `LLMConfig` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLMConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `AVAILABLE_LLMS`. Re-executing this cell updates kernel state.
- Store expression result in `LLMProvider`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/llm_models.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/llm_models.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | class |
| 24 | `LLMManager` | class |
| 19 | `__init__` | function |
| 32 | `__init__` | function |
| 36 | `_create_llm_instance` | function |
| 74 | `get_llm` | function |
| 92 | `get_model` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLM`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LLMManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm_manager`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._instances.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/routing.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/routing.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `build_agents` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `chat` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `build_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `chat(message, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `chatInterface`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `chatInterface.launch`: Start the UI/server; defining the UI alone does not start it.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/3-Routing/schemas.py](../../../../../../2_openai/community_contributions/workflow_patterns/3-Routing/schemas.py)

**Imports:**

- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `TopicCheckOutput` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TopicCheckOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/build_agents.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/build_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `llm_models` — Local or third-party dependency; follow its definition/import before execution.
- `schemas` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 90 | `check_if_on_topic` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `llm_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `available_model`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_TOKENS`. Re-executing this cell updates kernel state.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- Store expression result in `guardrail_topic_checker_agent`. Re-executing this cell updates kernel state.
- Define `check_if_on_topic(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `routing_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/configs.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/configs.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `LLMConfig` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLMConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `AVAILABLE_LLMS`. Re-executing this cell updates kernel state.
- Store expression result in `LLMProvider`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/google_search_agent.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/google_search_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `bs4` — Local or third-party dependency; follow its definition/import before execution.
- `googleapiclient.discovery` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.
- `schemas` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `fetch_page_content` | function |
| 23 | `run_google_search` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `bs4`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `googleapiclient.discovery`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `fetch_page_content(url)`. Its body runs when called.
- Define `run_google_search(query)`. Its body runs when called.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `service.cse(...).list(...).execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `res.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `search_results.results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/llm_models.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/llm_models.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | class |
| 24 | `LLMManager` | class |
| 19 | `__init__` | function |
| 32 | `__init__` | function |
| 36 | `_create_llm_instance` | function |
| 74 | `get_llm` | function |
| 92 | `get_model` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLM`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LLMManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm_manager`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._instances.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/orchestration_worker.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/orchestration_worker.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `build_agents` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `rich.console` — Formatted console output.
- `rich.markdown` — Formatted console output.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `ResearchManager` | class |
| 95 | `main` | async function |
| 23 | `run` | async function |
| 38 | `_topic_check` | async function |
| 69 | `_plan_searches` | async function |
| 74 | `_perform_searches` | async function |
| 81 | `_search` | async function |
| 86 | `_write_report` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `build_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.console`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.markdown`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `console`. Re-executing this cell updates kernel state.
- Define class `ResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `console.print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/schemas.py](../../../../../../2_openai/community_contributions/workflow_patterns/4-Orchestration_Workers/schemas.py)

**Imports:**

- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `SearchResult` | class |
| 10 | `SearchResults` | class |
| 14 | `WebSearchItem` | class |
| 21 | `WebSearchPlan` | class |
| 27 | `SyllabusData` | class |
| 37 | `TopicCheckOutput` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SearchResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SearchResults` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SyllabusData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TopicCheckOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/__init__.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/build_agents.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/build_agents.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `llm_models` — Local or third-party dependency; follow its definition/import before execution.
- `schemas` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `llm_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `available_model`. Re-executing this cell updates kernel state.
- Store expression result in `exercise_generator`. Re-executing this cell updates kernel state.
- Store expression result in `evaluator`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/configs.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/configs.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `LLMConfig` | class |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLMConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `AVAILABLE_LLMS`. Re-executing this cell updates kernel state.
- Store expression result in `LLMProvider`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/evaluator.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/evaluator.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `build_agents` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `rich.console` — Formatted console output.
- `rich.markdown` — Formatted console output.
- `schemas` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `build_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.console`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rich.markdown`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `console`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `console.print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `input_items.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/llm_models.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/llm_models.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `configs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | class |
| 24 | `LLMManager` | class |
| 19 | `__init__` | function |
| 32 | `__init__` | function |
| 36 | `_create_llm_instance` | function |
| 74 | `get_llm` | function |
| 92 | `get_model` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `configs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LLM`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LLMManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm_manager`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._instances.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/schemas.py](../../../../../../2_openai/community_contributions/workflow_patterns/5-Evaluator-Optimizer/schemas.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `EvaluationFeedback` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EvaluationFeedback` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
