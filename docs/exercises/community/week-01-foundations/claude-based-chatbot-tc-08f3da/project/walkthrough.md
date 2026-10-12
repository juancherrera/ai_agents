# claude based chatbot tc: project walkthrough

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

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/claude_based_chatbot_tc/README.md](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/README.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/claude_based_chatbot_tc/requirements.txt](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/claude_based_chatbot_tc/app.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/app.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `modules.chat` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `chat_wrapper` | function |
| 21 | `main` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modules.chat`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `chat_wrapper(message, history, state)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `chat_interface.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/__init__.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/chat.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/chat.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.data_loader` — Local or third-party dependency; follow its definition/import before execution.
- `.tools` — Local or third-party dependency; follow its definition/import before execution.
- `anthropic` — Native Claude client.
- `collections` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `sanitize_input` | function |
| 20 | `create_system_prompt` | function |
| 47 | `chat_function` | function |

**Read in this order:**

- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `collections`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.data_loader`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `anthropic_client`. Re-executing this cell updates kernel state.
- Define `sanitize_input(text)`. Its body runs when called.
- Define `create_system_prompt(name, summary, linkedin)`. Its body runs when called.
- Define `chat_function(message, history, state)`. Its body runs when called.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `anthropic_client.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `messages.extend`: Add every item from an iterable to a list.
- `tool_calls.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/config.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/config.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MODEL_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_TOKENS`. Re-executing this cell updates kernel state.
- Store expression result in `RATE_LIMIT`. Re-executing this cell updates kernel state.
- Store expression result in `DEFAULT_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `PUSHOVER_USER`. Re-executing this cell updates kernel state.
- Store expression result in `PUSHOVER_TOKEN`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/data_loader.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/data_loader.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pypdf` — Embedded PDF-text extraction.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `load_linkedin_pdf` | function |
| 26 | `load_text_file` | function |
| 41 | `load_personal_data` | function |

**Read in this order:**

- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `load_linkedin_pdf(filename, paths)`. Its body runs when called.
- Define `load_text_file(filename, paths)`. Its body runs when called.
- Define `load_personal_data()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/notification.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/notification.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `push` | function |

**Read in this order:**

- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `push(text)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/claude_based_chatbot_tc/modules/tools.py](../../../../../../1_foundations/community_contributions/claude_based_chatbot_tc/modules/tools.py)

**Imports:**

- `.notification` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `record_user_details` | function |
| 13 | `record_unknown_question` | function |
| 18 | `search_faq` | function |
| 69 | `handle_tool_calls` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.notification`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `record_user_details(email, name, notes)`. Its body runs when called.
- Define `record_unknown_question(question)`. Its body runs when called.
- Define `search_faq(query)`. Its body runs when called.
- Store list in `tool_schemas`. Re-executing this cell updates kernel state.
- Store dictionary in `tool_functions`. Re-executing this cell updates kernel state.
- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `tool_functions.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
