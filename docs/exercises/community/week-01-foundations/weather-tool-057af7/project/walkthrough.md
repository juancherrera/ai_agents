# weather tool: project walkthrough

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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/weather-tool/README.md](../../../../../../1_foundations/community_contributions/weather-tool/README.md): Weather Tool – Personal Assistant with Weather Integration. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/weather-tool/requirements.txt](../../../../../../1_foundations/community_contributions/weather-tool/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/weather-tool/app.py](../../../../../../1_foundations/community_contributions/weather-tool/app.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `openmeteo_requests` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pypdf` — Embedded PDF-text extraction.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `push` | function |
| 26 | `get_weather` | function |
| 82 | `record_user_details` | function |
| 86 | `record_unknown_question` | function |
| 136 | `Geocoding` | class |
| 175 | `Me` | class |
| 140 | `__init__` | function |
| 146 | `coordinates_search` | function |
| 177 | `__init__` | function |
| 189 | `handle_tool_call` | function |
| 200 | `system_prompt` | function |
| 229 | `chat` | function |

**Read in this order:**

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openmeteo_requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `push(text)`. Its body runs when called.
- Store expression result in `openmeteo`. Re-executing this cell updates kernel state.
- Define `get_weather(place_name, countryCode)`. Its body runs when called.
- Store dictionary in `get_weather_json`. Re-executing this cell updates kernel state.
- Define `record_user_details(email, name, notes)`. Its body runs when called.
- Define `record_unknown_question(question)`. Its body runs when called.
- Store dictionary in `record_user_details_json`. Re-executing this cell updates kernel state.
- Store dictionary in `record_unknown_question_json`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Define class `Geocoding`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Me`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `gr.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
