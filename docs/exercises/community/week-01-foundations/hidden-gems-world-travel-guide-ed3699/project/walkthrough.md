# hidden gems world travel guide: project walkthrough

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/hidden_gems_world_travel_guide/.github/workflows/update_space.yml](../../../../../../1_foundations/community_contributions/hidden_gems_world_travel_guide/.github/workflows/update_space.yml): configuration sections `name`, `on`, `jobs`. Match these names with code references and supplied input placeholders.
- [1_foundations/community_contributions/hidden_gems_world_travel_guide/README.md](../../../../../../1_foundations/community_contributions/hidden_gems_world_travel_guide/README.md): Hidden Gems World Travel Guide (RAG). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/hidden_gems_world_travel_guide/requirements.txt](../../../../../../1_foundations/community_contributions/hidden_gems_world_travel_guide/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/hidden_gems_world_travel_guide/app.py](../../../../../../1_foundations/community_contributions/hidden_gems_world_travel_guide/app.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `glob` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `math` — Local or third-party dependency; follow its definition/import before execution.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 38 | `VectorStore` | class |
| 64 | `HiddenGemsRAG` | class |
| 318 | `evaluate_with_anthropic` | function |
| 376 | `build_ui` | function |
| 40 | `__init__` | function |
| 44 | `add` | function |
| 48 | `build` | function |
| 55 | `search` | function |
| 66 | `__init__` | function |
| 72 | `infer_site_fields` | function |
| 194 | `_openai_post` | function |
| 206 | `_read_guides` | function |
| 218 | `_chunk_markdown` | function |
| 248 | `_embed` | function |
| 252 | `_load_and_index` | function |
| 266 | `_compose_system` | function |
| 276 | `retrieve` | function |
| 280 | `answer` | function |
| 382 | `respond` | function |
| 74 | `normalize_field` | function |
| 178 | `sort_key` | function |
| 463 | `on_send` | function |

**Read in this order:**

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `glob`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `math`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OPENAI_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `EMBEDDING_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `OPENAI_BASE`. Re-executing this cell updates kernel state.
- Store expression result in `OPENAI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_BASE`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ALLOWED_COUNTRIES`. Re-executing this cell updates kernel state.
- Store expression result in `ALLOWED_COUNTRIES_LOWER`. Re-executing this cell updates kernel state.
- Define class `VectorStore`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `HiddenGemsRAG`. Separate the class definition, constructing an instance, and calling its methods.
- Define `evaluate_with_anthropic(question, answer, history, sources, known_countries, requested_country)`. Its body runs when called.
- Define `build_ui(app)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `json.dumps`: Convert Python data to JSON text.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `out.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.documents.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `context_blocks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sources.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `deduped.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `chunks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `texts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `metas.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `p.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `clean_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `evaluation.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `rows.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `contents.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `seen.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/hidden_gems_world_travel_guide/hidden_gem_finder.py](../../../../../../1_foundations/community_contributions/hidden_gems_world_travel_guide/hidden_gem_finder.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 52 | `query_openai` | function |
| 65 | `generate_guides` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai.api_key`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store dictionary in `COUNTRIES_BY_CONTINENT`. Re-executing this cell updates kernel state.
- Store expression result in `OUTPUT_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `PROMPT_TEMPLATE`. Re-executing this cell updates kernel state.
- Define `query_openai(country)`. Its body runs when called.
- Define `generate_guides()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
