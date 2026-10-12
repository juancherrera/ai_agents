# osebas15: project walkthrough

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Configuration and supporting files

- [1_foundations/community_contributions/osebas15/day2_5_transcript_summary.md](../../../../../../1_foundations/community_contributions/osebas15/day2_5_transcript_summary.md): Day 2 Part 5: Workflow Design Patterns Summary. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/osebas15/basic_lab_setup.py](../../../../../../1_foundations/community_contributions/osebas15/basic_lab_setup.py)

**Imports:**

- `IPython.display` — Notebook display/shell facilities.
- `anthropic` — Native Claude client.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 30 | `setup` | function |
| 110 | `get_available_clients` | function |
| 130 | `get_client` | function |
| 143 | `get_default_model` | function |
| 157 | `create_completion` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai_client`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_client`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_client`. Re-executing this cell updates kernel state.
- Store expression result in `google_client`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_client`. Re-executing this cell updates kernel state.
- Store expression result in `groq_client`. Re-executing this cell updates kernel state.
- Store dictionary in `DEFAULT_MODELS`. Re-executing this cell updates kernel state.
- Define `setup()`. Its body runs when called.
- Define `get_available_clients()`. Its body runs when called.
- Define `get_client(provider)`. Its body runs when called.
- Define `get_default_model(provider)`. Its body runs when called.
- Define `create_completion(provider, messages, model)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `clients.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `available_clients.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `clients.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `client.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `kwargs.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
