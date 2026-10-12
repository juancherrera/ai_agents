# emotion and mcp: project walkthrough

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

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### A recurring engine is different from a UI

The UI displays state. An engine repeatedly invokes workers. A finite practice loop makes the scheduling visible without leaving a background job running.

**Ask yourself:** How many worker invocations happen with two rounds and four traders?

**Practice:** Add a market-open boolean that skips a round. Explain why the source while True needs an intentional stop method.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [6_mcp/community_contributions/emotion_and_mcp/README.md](../../../../../../6_mcp/community_contributions/emotion_and_mcp/README.md): Go MCP Server + Python Emotion API (Demo, CPU-Friendly). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [6_mcp/community_contributions/emotion_and_mcp/src/python_server/requirements.txt](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/python_server/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.
- [6_mcp/community_contributions/emotion_and_mcp/src/README.md](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/README.md): Emotion Detection MCP Server. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [6_mcp/community_contributions/emotion_and_mcp/src/main.py](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/main.py)

**Imports:**

- `connexion` — Local or third-party dependency; follow its definition/import before execution.
- `flask` — Local or third-party dependency; follow its definition/import before execution.
- `logging` — Application diagnostics.
- `torch` — Local or third-party dependency; follow its definition/import before execution.
- `torch.nn.functional` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.
- `unsloth` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `classify_emotion` | function |
| 57 | `load_model` | function |
| 79 | `predict` | function |

**Read in this order:**

- Make available: `unsloth`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `connexion`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `torch`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `torch.nn.functional`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `flask`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MODEL_PATH`. Re-executing this cell updates kernel state.
- Store list in `LABELS`. Re-executing this cell updates kernel state.
- Store expression result in `EMOJI`. Re-executing this cell updates kernel state.
- Define `classify_emotion(model_for_gen, tokenizer, text)`. Its body runs when called.
- Define `load_model()`. Its body runs when called.
- Define `predict(input_data)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `EMOJI.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `input_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `app.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emotion_and_mcp/src/python_server/emotion_server.py](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/python_server/emotion_server.py)

**Imports:**

- `flask` — Local or third-party dependency; follow its definition/import before execution.
- `logging` — Application diagnostics.
- `os` — Process configuration and operating-system paths.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.
- `transformers` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 35 | `load_emotion_model` | function |
| 52 | `health` | function |
| 60 | `predict` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `flask`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `transformers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `emotion_analyzer`. Re-executing this cell updates kernel state.
- Store dictionary in `EMOTION_EMOJI_MAP`. Re-executing this cell updates kernel state.
- Define `load_emotion_model()`. Its body runs when called.
- Define `health()`. Its body runs when called.
- Define `predict()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `app.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `app.post`: Send an HTTP POST. Notification/email calls have external effects.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `app.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `res.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `EMOTION_EMOJI_MAP.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emotion_and_mcp/src/tests/test_emotion_server.py](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/tests/test_emotion_server.py)

**Imports:**

- `requests` — Synchronous HTTP requests.
- `sys` — Interpreter/process information.

**Read in this order:**

- Make available: `requests`, `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BASE`. Re-executing this cell updates kernel state.
- Store expression result in `r`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emotion_and_mcp/src/tests/test_mcp_server.py](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/tests/test_mcp_server.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `queue` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `sys` — Interpreter/process information.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `urllib.parse` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `_post_with_retry` | function |
| 35 | `test_mcp_connection` | function |
| 52 | `sse_reader` | function |

**Read in this order:**

- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `urllib.parse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_post_with_retry(endpoint, body, retries, delay_seconds)`. Its body runs when called.
- Define `test_mcp_connection(message)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `threading.Thread(...).start`: Start the receiver runtime/server/resource.
- `session_id_container.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `response.close`: Release the resource; match setup with cleanup.
- `q.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `event_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `content.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emotion_and_mcp/src/ui.py](../../../../../../6_mcp/community_contributions/emotion_and_mcp/src/ui.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `queue` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `sys` — Interpreter/process information.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `urllib.parse` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `_post_with_retry` | function |
| 46 | `sse_reader` | function |
| 95 | `_call_direct_api` | function |
| 108 | `process_message` | function |
| 188 | `start_sse_thread` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `urllib.parse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MCP_BASE`. Re-executing this cell updates kernel state.
- Store expression result in `DIRECT_API_BASE`. Re-executing this cell updates kernel state.
- Store expression result in `q`. Re-executing this cell updates kernel state.
- Store expression result in `session_id_event`. Re-executing this cell updates kernel state.
- Store dictionary in `session_id_container`. Re-executing this cell updates kernel state.
- Store expression result in `sse_thread`. Re-executing this cell updates kernel state.
- Define `_post_with_retry(endpoint, body, retries, delay_seconds)`. Its body runs when called.
- Define `sse_reader(base_url)`. Its body runs when called.
- Define `_call_direct_api(text)`. Its body runs when called.
- Define `process_message(input_text, api_choice)`. Its body runs when called.
- Define `start_sse_thread()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `sse_thread.start`: Start the receiver runtime/server/resource.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `session_id_container.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `response.close`: Release the resource; match setup with cleanup.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `response_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `q.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `event_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `content.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
