# simple tools usage: project walkthrough

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

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

## Configuration and supporting files

- [1_foundations/community_contributions/simple-tools-usage/pyproject.toml](../../../../../../1_foundations/community_contributions/simple-tools-usage/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [1_foundations/community_contributions/simple-tools-usage/README.md](../../../../../../1_foundations/community_contributions/simple-tools-usage/README.md): Explanation and external references (remote contents unverified). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/simple-tools-usage/main.py](../../../../../../1_foundations/community_contributions/simple-tools-usage/main.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `re` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `smart_capitalize` | function |
| 16 | `manipulate_string` | function |
| 44 | `handle_tool_calls` | function |
| 73 | `chat` | function |
| 90 | `main` | function |

**Read in this order:**

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`, `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `call_to_action`. Re-executing this cell updates kernel state.
- Define `smart_capitalize(word)`. Its body runs when called.
- Define `manipulate_string(input_string)`. Its body runs when called.
- Store dictionary in `manipulate_string_json`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Store dictionary in `TOOL_FUNCTIONS`. Re-executing this cell updates kernel state.
- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- Store formatted string in `system_prompt`. Re-executing this cell updates kernel state.
- Define `chat(message, history)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `TOOL_FUNCTIONS.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `json.dumps`: Convert Python data to JSON text.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
