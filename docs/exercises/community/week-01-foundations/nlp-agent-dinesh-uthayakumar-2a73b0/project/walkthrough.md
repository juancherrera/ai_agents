# NLP Agent Dinesh Uthayakumar: project walkthrough

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [1_foundations/community_contributions/NLP_Agent_Dinesh_Uthayakumar/conversation-window.py](../../../../../../1_foundations/community_contributions/NLP_Agent_Dinesh_Uthayakumar/conversation-window.py)

**Imports:**

- `gtts` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.
- `scipy.io.wavfile` — Local or third-party dependency; follow its definition/import before execution.
- `sounddevice` — Local or third-party dependency; follow its definition/import before execution.
- `subprocess` — Launch/manage another process.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.
- `whisper` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 38 | `record_audio` | function |
| 47 | `transcribe_audio` | function |
| 60 | `transcribe_audio2` | function |
| 75 | `get_intent` | function |
| 87 | `get_llm_response` | function |
| 99 | `get_zoho_outstanding` | function |
| 110 | `get_dataverse_open_opportunities` | function |
| 123 | `speak2` | function |
| 130 | `speak` | function |
| 137 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sounddevice`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `whisper`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `scipy.io.wavfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gtts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OPENAI_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ZOHO_AUTH_TOKEN`. Re-executing this cell updates kernel state.
- Store expression result in `ZOHO_ORG_ID`. Re-executing this cell updates kernel state.
- Store expression result in `DATAVERSE_ENV`. Re-executing this cell updates kernel state.
- Store expression result in `DATAVERSE_TOKEN`. Re-executing this cell updates kernel state.
- Store expression result in `DURATION`. Re-executing this cell updates kernel state.
- Store expression result in `FS`. Re-executing this cell updates kernel state.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Define `record_audio(filename)`. Its body runs when called.
- Define `transcribe_audio(filename)`. Its body runs when called.
- Define `transcribe_audio2(filename)`. Its body runs when called.
- Define `get_intent(text)`. Its body runs when called.
- Define `get_llm_response(text)`. Its body runs when called.
- Define `get_zoho_outstanding()`. Its body runs when called.
- Define `get_dataverse_open_opportunities()`. Its body runs when called.
- Define `speak2(text)`. Its body runs when called.
- Define `speak(text)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `write`: Write data to a file-like object; inspect the path and mode.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `op.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `inv.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
