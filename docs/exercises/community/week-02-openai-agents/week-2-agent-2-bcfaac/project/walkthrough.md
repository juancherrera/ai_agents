# week 2 agent (2).py: project walkthrough

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

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Module-by-module reading map

### [2_openai/community_contributions/week_2_agent (2).py](../../../../../../2_openai/community_contributions/week_2_agent%20%282%29.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `google.colab` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `nest_asyncio` — Nested-loop patching.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.lib.pagesizes` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.lib.styles` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.platypus` — Local or third-party dependency; follow its definition/import before execution.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 61 | `call_anthropic` | function |
| 74 | `ClarifierOutput` | class |
| 78 | `ClarifierAgent` | class |
| 99 | `KeyFinding` | class |
| 103 | `ResearcherOutput` | class |
| 109 | `ResearcherAgent` | class |
| 142 | `EvaluationOutput` | class |
| 150 | `EvaluatorAgent` | class |
| 184 | `ResearchManager` | class |
| 212 | `generate_pdf` | function |
| 257 | `process_query` | async function |
| 268 | `on_generate_click` | async function |
| 79 | `run` | async function |
| 110 | `run` | async function |
| 151 | `run` | async function |
| 185 | `__init__` | function |
| 190 | `run` | async function |
| 221 | `section` | function |

**Read in this order:**

- Make available: `os`, `json`, `asyncio`, `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `nest_asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.lib.pagesizes`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.platypus`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.lib.styles`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.colab`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ANTHROPIC_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_BASE_URL`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `completion`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Define `call_anthropic(prompt, temperature, max_tokens)`. Its body runs when called.
- Define class `ClarifierOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ClarifierAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `KeyFinding` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ResearcherOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ResearcherAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluationOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluatorAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ResearchManager`. Separate the class definition, constructing an instance, and calling its methods.
- Define `generate_pdf(report, prefix)`. Its body runs when called.
- Define `process_query(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `on_generate_click(q)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `userdata.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `elems.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `self.clarifier.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.researcher.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.evaluator.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
