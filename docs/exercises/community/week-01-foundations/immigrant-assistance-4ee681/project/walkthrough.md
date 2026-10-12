# immigrant assistance: project walkthrough

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

## Configuration and supporting files

- [1_foundations/community_contributions/immigrant-assistance/pyproject.toml](../../../../../../1_foundations/community_contributions/immigrant-assistance/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.

## Module-by-module reading map

### [1_foundations/community_contributions/immigrant-assistance/main.py](../../../../../../1_foundations/community_contributions/immigrant-assistance/main.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 46 | `getting_drivers_license` | function |
| 58 | `opening_savings_account_or_checking_account` | function |
| 69 | `find_and_rent_apartment` | function |
| 81 | `buying_a_car_json` | function |
| 94 | `types_of_visas_json` | function |
| 104 | `main` | function |
| 157 | `chat` | function |
| 182 | `handle_tool_calls` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Define `getting_drivers_license()`. Its body runs when called.
- Define `opening_savings_account_or_checking_account()`. Its body runs when called.
- Define `find_and_rent_apartment()`. Its body runs when called.
- Define `buying_a_car_json()`. Its body runs when called.
- Define `types_of_visas_json()`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `gr.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
