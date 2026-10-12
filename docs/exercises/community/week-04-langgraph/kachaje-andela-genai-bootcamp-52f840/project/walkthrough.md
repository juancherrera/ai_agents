# kachaje andela genai bootcamp: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Build a real local LangGraph without a model

A graph is state plus nodes plus edges. Nodes return state updates. Compilation builds the executable workflow; invoke runs it. This exercises the installed LangGraph package locally.

**Ask yourself:** Which value will the uppercase node return?

**Practice:** Add a second node that appends an exclamation mark. Wire it between uppercase and END.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Conditional routing makes the next step explicit

A route is a function of state. A worker may need tools, evaluation, another attempt, or user clarification. This exercise checks all branch labels without calling any model.

**Ask yourself:** Which route wins when a tool call is present?

**Practice:** Add an attempts field and route to END after a fixed limit. Explain why asking for clarification is a valid stop condition.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/README.md](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/README.md): CodeBuilder. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/__init__.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/file_creator.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/file_creator.py)

**Imports:**

- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `create_project` | function |
| 223 | `_extract_file_names_from_plan` | function |
| 282 | `_get_extension` | function |

**Read in this order:**

- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `create_project(plan, language, session_id)`. Its body runs when called.
- Define `_extract_file_names_from_plan(plan, language)`. Its body runs when called.
- Define `_get_extension(language)`. Its body runs when called.
- `extensions.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `code_blocks_with_names.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `readme.write_text`: Write a text file at the selected path.
- `files_created.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `simple_code_blocks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `main_file.write_text`: Write a text file at the selected path.
- `file_names.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `match.start`: Start the receiver runtime/server/resource.
- `filepath.write_text`: Write a text file at the selected path.
- `files_skipped.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/llm_client.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/llm_client.py)

**Imports:**

- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `ollama` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `generate_plan` | function |

**Read in this order:**

- Make available: `ollama`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `generate_plan(challenge, language, search_results)`. Its body runs when called.
- `response.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/main.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/main.py)

**Imports:**

- `backend.file_creator` — Local or third-party dependency; follow its definition/import before execution.
- `backend.models` — Local or third-party dependency; follow its definition/import before execution.
- `backend.state_manager` — Local or third-party dependency; follow its definition/import before execution.
- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `fastapi` — Local or third-party dependency; follow its definition/import before execution.
- `fastapi.middleware.cors` — Local or third-party dependency; follow its definition/import before execution.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 28 | `create_session` | async function |
| 50 | `submit_challenge` | async function |
| 129 | `get_state` | async function |
| 162 | `build_project` | async function |
| 240 | `clear_session` | async function |

**Read in this order:**

- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fastapi`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fastapi.middleware.cors`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.state_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `api`. Re-executing this cell updates kernel state.
- Store dictionary in `sessions`. Re-executing this cell updates kernel state.
- Define `create_session()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `submit_challenge(session_id, request)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_state(session_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `build_project(session_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `clear_session(session_id)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `api.post`: Send an HTTP POST. Notification/email calls have external effects.
- `api.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `build_result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/models.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/models.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `SessionRequest` | class |
| 9 | `ChallengeRequest` | class |
| 13 | `StateResponse` | class |
| 22 | `BuildResponse` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SessionRequest` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ChallengeRequest` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `StateResponse` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `BuildResponse` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/state_manager.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/state_manager.py)

**Imports:**

- `backend.file_creator` — Local or third-party dependency; follow its definition/import before execution.
- `backend.llm_client` — Local or third-party dependency; follow its definition/import before execution.
- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `backend.web_search` — Local or third-party dependency; follow its definition/import before execution.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `State` | class |
| 20 | `challenge_node` | function |
| 41 | `plan_node` | function |
| 78 | `build_node` | function |
| 114 | `router_node` | function |
| 118 | `route_next_step` | function |
| 128 | `execute_plan_workflow` | function |
| 146 | `execute_build_workflow` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.web_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.llm_client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.file_creator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `challenge_node(state)`. Its body runs when called.
- Define `plan_node(state)`. Its body runs when called.
- Define `build_node(state)`. Its body runs when called.
- Define `router_node(state)`. Its body runs when called.
- Define `route_next_step(state)`. Its body runs when called.
- Define `execute_plan_workflow(state)`. Its body runs when called.
- Define `execute_build_workflow(state)`. Its body runs when called.
- Store expression result in `workflow`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `workflow.add_node`: Register a state-processing function under a node name.
- `workflow.add_conditional_edges`: Choose the next path from state using a routing function.
- `workflow.add_edge`: Connect one node to the next in a fixed path.
- `workflow.compile`: Build an executable graph or compiled object; this is separate from running it.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/utils/__init__.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/utils/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/utils/logger.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/utils/logger.py)

**Imports:**

- `colorlog` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `pathlib` — Explicit filesystem paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `CodeBuilderLogger` | class |
| 323 | `get_logger` | function |
| 9 | `__init__` | function |
| 56 | `log_api_request` | function |
| 117 | `log_llm_call` | function |
| 165 | `log_file_operation` | function |
| 202 | `log_state_transition` | function |
| 228 | `log_web_search` | function |
| 264 | `log_session_event` | function |
| 285 | `log_workflow_node` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `colorlog`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CodeBuilderLogger`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `_logger_instance`. Re-executing this cell updates kernel state.
- Define `get_logger()`. Its body runs when called.
- `log_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/web_search.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/backend/web_search.py)

**Imports:**

- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `duckduckgo_search` — Local or third-party dependency; follow its definition/import before execution.
- `duckduckgo_search.exceptions` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `search_code_examples` | function |

**Read in this order:**

- Make available: `duckduckgo_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `duckduckgo_search.exceptions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_RETRIES`. Re-executing this cell updates kernel state.
- Store expression result in `BASE_DELAY`. Re-executing this cell updates kernel state.
- Define `search_code_examples(query, language)`. Its body runs when called.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/frontend/__init__.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/frontend/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/frontend/gradio_ui.py](../../../../../../4_langgraph/community_contributions/kachaje-andela-genai-bootcamp/frontend/gradio_ui.py)

**Imports:**

- `backend.utils.logger` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `requests` — Synchronous HTTP requests.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `get_state_label` | function |
| 26 | `get_input_placeholder` | function |
| 36 | `run_action` | function |
| 170 | `clear_session` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `backend.utils.logger`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `API_BASE`. Re-executing this cell updates kernel state.
- Store expression result in `session_id`. Re-executing this cell updates kernel state.
- Store expression result in `current_state`. Re-executing this cell updates kernel state.
- Store expression result in `plan_text`. Re-executing this cell updates kernel state.
- Store list in `build_messages`. Re-executing this cell updates kernel state.
- Define `get_state_label(state)`. Its body runs when called.
- Define `get_input_placeholder(state)`. Its body runs when called.
- Define `run_action(input_text, output_text)`. Its body runs when called.
- Define `clear_session()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `labels.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `placeholders.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
