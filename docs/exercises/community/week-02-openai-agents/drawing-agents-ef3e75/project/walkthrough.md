# drawing agents: project walkthrough

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

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Text and image inputs are different content items

A multimodal message can contain text plus an image. A structured description can constrain fields such as orientation. This local example constructs a tiny image and validates a description without a model.

**Ask yourself:** Which orientation is valid for an image wider than it is tall?

**Practice:** Create a square image and update orientation. Explain that valid orientation syntax alone does not establish what a model saw.

## Configuration and supporting files

- [2_openai/community_contributions/drawing_agents/pyproject.toml](../../../../../../2_openai/community_contributions/drawing_agents/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [2_openai/community_contributions/drawing_agents/README.md](../../../../../../2_openai/community_contributions/drawing_agents/README.md): openai-drawing-agents. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/drawing_agents/main.py](../../../../../../2_openai/community_contributions/drawing_agents/main.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `context.app_context` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `my_agents.drawing_evaluation_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.drawing_planning_agent` — Local or third-party dependency; follow its definition/import before execution.
- `tools.graphic_canvas` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `context.app_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.drawing_evaluation_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.drawing_planning_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.graphic_canvas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OUTPUT_FILE`. Re-executing this cell updates kernel state.
- Store expression result in `DRAW_REQUEST`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/constants.py](../../../../../../2_openai/community_contributions/drawing_agents/src/constants.py)

**Read in this order:**

- Store expression result in `CANVAS_WIDTH`, `CANVAS_HEIGHT`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/context/app_context.py](../../../../../../2_openai/community_contributions/drawing_agents/src/context/app_context.py)

**Imports:**

- `dataclasses` — Simple typed records.
- `tools.graphic_canvas` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `AppContext` | class |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.graphic_canvas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AppContext`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/my_agents/drawing_agent.py](../../../../../../2_openai/community_contributions/drawing_agents/src/my_agents/drawing_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `context.app_context` — Local or third-party dependency; follow its definition/import before execution.
- `tools.graphic_tool` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `context.app_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.graphic_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `drawing_agent`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/my_agents/drawing_evaluation_agent.py](../../../../../../2_openai/community_contributions/drawing_agents/src/my_agents/drawing_evaluation_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `openai.types.responses` — Compatible model clients and request/response types.
- `openai.types.responses.response_input_item_param` — Compatible model clients and request/response types.
- `utils.image_utils` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `run_evaluation_agent` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai.types.responses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai.types.responses.response_input_item_param`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils.image_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `drawing_evaluation_agent`. Re-executing this cell updates kernel state.
- Define `run_evaluation_agent(text, image_path)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/my_agents/drawing_planning_agent.py](../../../../../../2_openai/community_contributions/drawing_agents/src/my_agents/drawing_planning_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `constants` — Local or third-party dependency; follow its definition/import before execution.
- `context.app_context` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.drawing_agent` — Local or third-party dependency; follow its definition/import before execution.
- `my_agents.input_guardrail_agent` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `context.app_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.drawing_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.input_guardrail_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `drawing_planning_agent`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/my_agents/input_guardrail_agent.py](../../../../../../2_openai/community_contributions/drawing_agents/src/my_agents/input_guardrail_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `context.app_context` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `DrawingRequestOutput` | class |
| 25 | `drawing_guardrail` | async function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `context.app_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `DrawingRequestOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `input_guardrail_agent`. Re-executing this cell updates kernel state.
- Define `drawing_guardrail(ctx, agent, input)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/tools/graphic_canvas.py](../../../../../../2_openai/community_contributions/drawing_agents/src/tools/graphic_canvas.py)

**Imports:**

- `PIL` — Image objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `_to_rgba` | function |
| 37 | `HeadlessCanvas` | class |
| 49 | `__init__` | function |
| 55 | `initialize` | function |
| 65 | `free_draw` | function |
| 91 | `save_and_cleanup` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `PIL`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ColorLike`. Re-executing this cell updates kernel state.
- Define `_to_rgba(color, alpha)`. Its body runs when called.
- Define class `HeadlessCanvas`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/tools/graphic_tool.py](../../../../../../2_openai/community_contributions/drawing_agents/src/tools/graphic_tool.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `context.app_context` — Local or third-party dependency; follow its definition/import before execution.
- `tools.graphic_canvas` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `free_draw_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `context.app_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.graphic_canvas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ColorLike`. Re-executing this cell updates kernel state.
- Define `free_draw_tool(ctx, path, color, brush_px)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/src/utils/image_utils.py](../../../../../../2_openai/community_contributions/drawing_agents/src/utils/image_utils.py)

**Imports:**

- `base64` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `encode_image_base64` | function |

**Read in this order:**

- Make available: `base64`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `encode_image_base64(path)`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/test.py](../../../../../../2_openai/community_contributions/drawing_agents/test.py)

**Imports:**

- `sys` — Interpreter/process information.

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/tests/test_evaluation.py](../../../../../../2_openai/community_contributions/drawing_agents/tests/test_evaluation.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `my_agents.drawing_evaluation_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `my_agents.drawing_evaluation_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OUTPUT_FILE`. Re-executing this cell updates kernel state.
- Store expression result in `DRAW_REQUEST`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/drawing_agents/tests/test_graphics.py](../../../../../../2_openai/community_contributions/drawing_agents/tests/test_graphics.py)

**Imports:**

- `tools.graphic_canvas` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `draw_house` | function |
| 42 | `draw_person` | function |
| 79 | `draw_dog` | function |
| 124 | `main` | function |

**Read in this order:**

- Make available: `tools.graphic_canvas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `GLOBAL_CANVAS`. Re-executing this cell updates kernel state.
- Define `draw_house()`. Its body runs when called.
- Define `draw_person()`. Its body runs when called.
- Define `draw_dog()`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
