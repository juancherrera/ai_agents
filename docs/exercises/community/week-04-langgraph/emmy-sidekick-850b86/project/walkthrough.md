# emmy sidekick: project walkthrough

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [4_langgraph/community_contributions/emmy_sidekick/pyproject.toml](../../../../../../4_langgraph/community_contributions/emmy_sidekick/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [4_langgraph/community_contributions/emmy_sidekick/README.md](../../../../../../4_langgraph/community_contributions/emmy_sidekick/README.md): 🎯 Interview Prep Agent. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [4_langgraph/community_contributions/emmy_sidekick/main.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/main.py)

**Imports:**

- `src.ui` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `main` | function |

**Read in this order:**

- Make available: `src.ui`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/__init__.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/__init__.py)

**Read in this order:**

- Store expression result in `__version__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/config.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/config.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `GROQ_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `TAVILY_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `YOUTUBE_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_TEMPERATURE`. Re-executing this cell updates kernel state.
- Store expression result in `TAVILY_MAX_RESULTS`. Re-executing this cell updates kernel state.
- Store expression result in `TAVILY_SEARCH_DEPTH`. Re-executing this cell updates kernel state.
- Store expression result in `TAVILY_TIMEOUT`. Re-executing this cell updates kernel state.
- Store expression result in `YOUTUBE_MAX_RESULTS`. Re-executing this cell updates kernel state.
- Store expression result in `YOUTUBE_API_URL`. Re-executing this cell updates kernel state.
- Store expression result in `YOUTUBE_TIMEOUT`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_REFINEMENTS`. Re-executing this cell updates kernel state.
- Store expression result in `UI_TITLE`. Re-executing this cell updates kernel state.
- Store expression result in `UI_SUBTITLE`. Re-executing this cell updates kernel state.
- Store expression result in `UI_HEIGHT`. Re-executing this cell updates kernel state.
- Store expression result in `UI_PLACEHOLDER`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/external_services.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/external_services.py)

**Imports:**

- `requests` — Synchronous HTTP requests.
- `src.config` — Local or third-party dependency; follow its definition/import before execution.
- `tavily` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `search_company_info` | function |
| 74 | `search_youtube` | function |

**Read in this order:**

- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tavily`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `search_company_info(company, role)`. Its body runs when called.
- Define `search_youtube(company, role)`. Its body runs when called.
- `response.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `content_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `videos.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `sources.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/graph.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/graph.py)

**Imports:**

- `langgraph.checkpoint.memory` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `src.models` — Local or third-party dependency; follow its definition/import before execution.
- `src.nodes` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `route_start` | function |
| 37 | `route_planner` | function |
| 55 | `route_refinement` | function |
| 77 | `build_graph` | function |

**Read in this order:**

- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.nodes`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `route_start(state)`. Its body runs when called.
- Define `route_planner(state)`. Its body runs when called.
- Define `route_refinement(state)`. Its body runs when called.
- Define `build_graph()`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `StateGraph`: Start a graph builder using the declared state schema.
- `builder.add_node`: Register a state-processing function under a node name.
- `builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `builder.add_edge`: Connect one node to the next in a fixed path.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `builder.compile`: Build an executable graph or compiled object; this is separate from running it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/llm.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/llm.py)

**Imports:**

- `langchain_groq` — Local or third-party dependency; follow its definition/import before execution.
- `src.config` — Local or third-party dependency; follow its definition/import before execution.
- `src.models` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `langchain_groq`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `planner_llm`. Re-executing this cell updates kernel state.
- `llm.with_structured_output`: Request a result matching the supplied schema.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/models.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/models.py)

**Imports:**

- `langgraph.graph.message` — State, nodes, routing, and checkpoints.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `State` | class |
| 19 | `PlannerOutput` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PlannerOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/nodes.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/nodes.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `src.config` — Local or third-party dependency; follow its definition/import before execution.
- `src.external_services` — Local or third-party dependency; follow its definition/import before execution.
- `src.llm` — Local or third-party dependency; follow its definition/import before execution.
- `src.models` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `planner` | function |
| 71 | `research` | function |
| 124 | `generate_guide` | function |
| 167 | `ask_refinement` | function |
| 206 | `handle_refinement` | function |
| 267 | `youtube_search_node` | function |

**Read in this order:**

- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.llm`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.external_services`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `planner(state)`. Its body runs when called.
- Define `research(state)`. Its body runs when called.
- Define `generate_guide(state)`. Its body runs when called.
- Define `ask_refinement(state)`. Its body runs when called.
- Define `handle_refinement(state)`. Its body runs when called.
- Define `youtube_search_node(state)`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `planner_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `search_results.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/emmy_sidekick/src/ui.py](../../../../../../4_langgraph/community_contributions/emmy_sidekick/src/ui.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `langchain_core.messages` — Message/tool interfaces.
- `src.config` — Local or third-party dependency; follow its definition/import before execution.
- `src.graph` — Local or third-party dependency; follow its definition/import before execution.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `reset` | function |
| 22 | `compat_message` | function |
| 36 | `chat` | async function |
| 78 | `create_app` | function |

**Read in this order:**

- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `THREAD_ID`. Re-executing this cell updates kernel state.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Define `reset()`. Its body runs when called.
- Define `compat_message(message, owner)`. Its body runs when called.
- Define `chat(message, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `create_app()`. Its body runs when called.
- `history.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
