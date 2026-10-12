# 2 lab2 Execution measurement.py: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

## Module-by-module reading map

### [1_foundations/community_contributions/2_lab2_Execution_measurement.py](../../../../../../1_foundations/community_contributions/2_lab2_Execution_measurement.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `concurrent.futures` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 45 | `key_checker` | function |
| 72 | `question_prompt_generator` | function |
| 78 | `generate_competition_question` | function |
| 90 | `llm_caller` | function |
| 97 | `llm_caller_with_model` | function |
| 116 | `get_single_model_answer` | function |
| 130 | `get_models_answers` | function |
| 145 | `get_models_answers_parallel` | function |
| 172 | `get_single_model_answer_async` | async function |
| 194 | `get_models_answers_async` | async function |
| 225 | `together_maker` | function |
| 232 | `judge_prompt_generator` | function |
| 249 | `judge_caller` | function |
| 260 | `compare_execution_methods` | function |
| 325 | `run_llm_competition` | function |
| 351 | `get_execution_method` | function |
| 387 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `concurrent.futures`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store list in `competitors`. Re-executing this cell updates kernel state.
- Store list in `answers`. Re-executing this cell updates kernel state.
- Store expression result in `together`. Re-executing this cell updates kernel state.
- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store dictionary in `models_dict`. Re-executing this cell updates kernel state.
- Define `key_checker()`. Its body runs when called.
- Define `question_prompt_generator()`. Its body runs when called.
- Define `generate_competition_question()`. Its body runs when called.
- Define `llm_caller(messages)`. Its body runs when called.
- Define `llm_caller_with_model(messages, model_name, api_key, base_url)`. Its body runs when called.
- Define `get_single_model_answer(provider, details, question_messages)`. Its body runs when called.
- Define `get_models_answers(question_messages)`. Its body runs when called.
- Define `get_models_answers_parallel(question_messages, max_workers)`. Its body runs when called.
- Define `get_single_model_answer_async(provider, details, question_messages)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_models_answers_async(question_messages)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `together_maker(answers)`. Its body runs when called.
- Define `judge_prompt_generator(competitors, question, together)`. Its body runs when called.
- Define `judge_caller(judge_prompt, competitors)`. Its body runs when called.
- Define `compare_execution_methods(question_messages, runs_per_method)`. Its body runs when called.
- Define `run_llm_competition(question_messages, execution_method, question)`. Its body runs when called.
- Define `get_execution_method()`. Its body runs when called.
- Define `main()`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `llm.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `method_times.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
