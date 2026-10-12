# basic page language detector: project walkthrough

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

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

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [4_langgraph/community_contributions/basic_page_language_detector/pyproject.toml](../../../../../../4_langgraph/community_contributions/basic_page_language_detector/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [4_langgraph/community_contributions/basic_page_language_detector/README.md](../../../../../../4_langgraph/community_contributions/basic_page_language_detector/README.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [4_langgraph/community_contributions/basic_page_language_detector/sidekick.py](../../../../../../4_langgraph/community_contributions/basic_page_language_detector/sidekick.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `langgraph.checkpoint.memory` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `langgraph.graph.message` — State, nodes, routing, and checkpoints.
- `langgraph.prebuilt` — State, nodes, routing, and checkpoints.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `sidekick_tools` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `typing_extensions` — Additional typing features.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `State` | class |
| 32 | `EvaluatorOutput` | class |
| 39 | `ManagerOutput` | class |
| 42 | `Sidekick` | class |
| 43 | `__init__` | function |
| 55 | `setup` | async function |
| 69 | `manager` | function |
| 88 | `language_worker` | function |
| 133 | `worker` | function |
| 174 | `workers_router` | function |
| 183 | `tool_router` | function |
| 186 | `manager_router` | function |
| 192 | `format_conversation` | function |
| 202 | `evaluator` | function |
| 232 | `route_based_on_evaluation` | function |
| 243 | `build_graph` | async function |
| 268 | `run_superstep` | async function |
| 307 | `cleanup` | function |
| 316 | `main` | async function |

**Read in this order:**

- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing_extensions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sidekick_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluatorOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ManagerOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Sidekick`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Field`: Describe a schema field or constrain allowed values.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `manager_llm.with_structured_output`: Request a result matching the supplied schema.
- `worker_llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `language_worker_llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `evaluator_llm.with_structured_output`: Request a result matching the supplied schema.
- `self.manager_llm_with_structured_output.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.language_worker_llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `self.worker_llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `self.evaluator_llm_with_output.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph_builder.add_node`: Register a state-processing function under a node name.
- `graph_builder.add_edge`: Connect one node to the next in a fixed path.
- `graph_builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph_builder.compile`: Build an executable graph or compiled object; this is separate from running it.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `ToolNode`: Create the graph node responsible for executing requested tools.
- `self.graph.ainvoke`: Run an asynchronous graph or model operation with await.
- `loop.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `self.browser.close`: Release the resource; match setup with cleanup.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/basic_page_language_detector/sidekick_tools.py](../../../../../../4_langgraph/community_contributions/basic_page_language_detector/sidekick_tools.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `bs4` — Local or third-party dependency; follow its definition/import before execution.
- `contextlib` — Resource lifetime helpers.
- `dotenv` — Load explicit environment-file configuration.
- `langchain.tools` — Model/tool wrappers.
- `langchain_community.agent_toolkits` — Search, browser, document, and file integrations.
- `langchain_community.tools` — Search, browser, document, and file integrations.
- `langchain_community.tools.playwright.utils` — Search, browser, document, and file integrations.
- `langchain_community.utilities` — Search, browser, document, and file integrations.
- `langchain_experimental.utilities` — Experimental tools including a Python REPL.
- `langdetect` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pandas` — Tabular data.
- `pathlib` — Explicit filesystem paths.
- `playwright.async_api` — Browser automation with setup/action/cleanup.
- `requests` — Synchronous HTTP requests.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `get_url_lang_data_redirect_auto` | function |
| 59 | `page_lang_detector` | function |
| 73 | `run_python` | function |
| 80 | `push_notification` | function |
| 87 | `wiki` | function |
| 93 | `search_web` | function |
| 97 | `setup_playwright` | async function |
| 108 | `file_management_tools` | function |
| 116 | `read_csv_snippet` | function |
| 131 | `all_tools` | async function |

**Read in this order:**

- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `bs4`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langdetect`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools.playwright.utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `playwright.async_api`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_experimental.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_url_lang_data_redirect_auto(url)`. Its body runs when called.
- Define `page_lang_detector(path)`. Its body runs when called.
- Store expression result in `python_repl`. Re-executing this cell updates kernel state.
- Define `run_python(code)`. Its body runs when called.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Define `push_notification(text)`. Its body runs when called.
- Store expression result in `wikipedia`. Re-executing this cell updates kernel state.
- Define `wiki(query)`. Its body runs when called.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Define `search_web(query)`. Its body runs when called.
- Define `setup_playwright()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `working_directory`. Re-executing this cell updates kernel state.
- Define `file_management_tools()`. Its body runs when called.
- Define `read_csv_snippet(file_path)`. Its body runs when called.
- Define `all_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `python_repl.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `wikipedia.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `serper.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `p.chromium.launch`: Start the UI/server; defining the UI alone does not start it.
- `html_tag.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/basic_page_language_detector/ui.py](../../../../../../4_langgraph/community_contributions/basic_page_language_detector/ui.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `os` — Process configuration and operating-system paths.
- `sidekick` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `setup` | async function |
| 10 | `process_message` | async function |
| 31 | `reset` | async function |
| 36 | `free_resources` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sidekick`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `setup()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `process_message(sidekick, message, success_criteria, history, input_csv_file)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `reset()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `free_resources(sidekick)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
