# jongkook: project walkthrough

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

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

## Configuration and supporting files

- [1_foundations/community_contributions/jongkook/README.md](../../../../../../1_foundations/community_contributions/jongkook/README.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/jongkook/requirements.txt](../../../../../../1_foundations/community_contributions/jongkook/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/jongkook/app.py](../../../../../../1_foundations/community_contributions/jongkook/app.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `pypdf` — Embedded PDF-text extraction.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `push` | function |
| 18 | `record_user_details` | function |
| 47 | `record_unknown_question` | function |
| 74 | `handle_tool_calls` | function |
| 105 | `Evaluation` | class |
| 121 | `avator` | function |
| 160 | `evaluator_user_prompt` | function |
| 167 | `evaluator` | function |
| 186 | `orchestrator` | function |

**Read in this order:**

- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Define `push(message)`. Its body runs when called.
- Define `record_user_details(email, name, notes)`. Its body runs when called.
- Store dictionary in `record_user_details_json`. Re-executing this cell updates kernel state.
- Define `record_unknown_question(question)`. Its body runs when called.
- Store dictionary in `record_unknown_question_json`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `linkedin`. Re-executing this cell updates kernel state.
- Store expression result in `linkedin_profile`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `name`. Re-executing this cell updates kernel state.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Evaluation` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store formatted string in `avator_system_prompt`. Re-executing this cell updates kernel state.
- Define `avator(message, history, evaluation)`. Its body runs when called.
- Store formatted string in `evaluator_system_prompt`. Re-executing this cell updates kernel state.
- Define `evaluator_user_prompt(question, avator_response, history)`. Its body runs when called.
- Define `evaluator(question, avator_response, history)`. Its body runs when called.
- Store expression result in `max_attempt`. Re-executing this cell updates kernel state.
- Define `orchestrator(message, history)`. Its body runs when called.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `gradio.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `llm_client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `OpenAI(...).chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `gradio.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
