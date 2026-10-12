# salah: project walkthrough

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

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/analyzer_tools.py](../../../../../../4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/analyzer_tools.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain.agents` — Model/tool wrappers.
- `langchain_community.agent_toolkits` — Search, browser, document, and file integrations.
- `langchain_community.tools.wikipedia.tool` — Search, browser, document, and file integrations.
- `langchain_community.utilities` — Search, browser, document, and file integrations.
- `langchain_experimental.tools` — Experimental tools including a Python REPL.
- `os` — Process configuration and operating-system paths.
- `playwright.async_api` — Browser automation with setup/action/cleanup.
- `requests` — Synchronous HTTP requests.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `AnalyzerTools` | class |
| 19 | `setup_playwright_tools` | async function |
| 27 | `setup_file_tools` | function |
| 32 | `setup_search_tool` | function |
| 43 | `setup_wikipedia_tool` | function |
| 49 | `setup_python_repl_tool` | function |
| 54 | `setup_notification_tool` | function |
| 91 | `setup_all_tools` | async function |
| 59 | `send_push_notification` | function |

**Read in this order:**

- Make available: `playwright.async_api`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools.wikipedia.tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_experimental.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AnalyzerTools`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `async_playwright(...).start`: Start the receiver runtime/server/resource.
- `playwright.chromium.launch`: Start the UI/server; defining the UI alone does not start it.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/app.py](../../../../../../4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/app.py)

**Imports:**

- `config` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `log_analyzer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `setup` | async function |
| 14 | `analyze` | async function |
| 25 | `reset` | async function |
| 30 | `cleanup_agent` | function |
| 38 | `update_config` | function |
| 73 | `refresh_log_files` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `log_analyzer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `setup()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `analyze(agent, log_file, criteria, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `reset()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `cleanup_agent(agent)`. Its body runs when called.
- Define `update_config(agent, config, logs_dir, source_dir, error_patterns, max_errors, enable_source, enable_notif, enable_browser)`. Its body runs when called.
- Define `refresh_log_files(agent)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/config.py](../../../../../../4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/config.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `LogAnalyzerConfig` | class |
| 54 | `get_absolute_logs_path` | function |
| 59 | `get_absolute_source_path` | function |
| 64 | `validate_directories` | function |
| 73 | `get_error_pattern_regex` | function |
| 76 | `Config` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `LogAnalyzerConfig` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/log_analyzer_agent.py](../../../../../../4_langgraph/community_contributions/salah/log-analyzer-and-alert-agent/log_analyzer_agent.py)

**Imports:**

- `analyzer_tools` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `langgraph.checkpoint.memory` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `langgraph.graph.message` — State, nodes, routing, and checkpoints.
- `langgraph.prebuilt` — State, nodes, routing, and checkpoints.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.
- `typing_extensions` — Additional typing features.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `State` | class |
| 27 | `EvaluatorOutput` | class |
| 33 | `LogAnalyzerAgent` | class |
| 34 | `__init__` | function |
| 45 | `setup` | async function |
| 60 | `worker` | function |
| 156 | `worker_router` | function |
| 162 | `format_conversation` | function |
| 172 | `evaluator` | function |
| 237 | `evaluation_router` | function |
| 242 | `build_graph` | async function |
| 263 | `analyze_log` | async function |
| 285 | `cleanup` | function |
| 297 | `get_log_files` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing_extensions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `analyzer_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluatorOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LogAnalyzerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `worker_llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `evaluator_llm.with_structured_output`: Request a result matching the supplied schema.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.worker_llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `self.evaluator_llm_with_output.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph_builder.add_node`: Register a state-processing function under a node name.
- `graph_builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph_builder.add_edge`: Connect one node to the next in a fixed path.
- `graph_builder.compile`: Build an executable graph or compiled object; this is separate from running it.
- `ToolNode`: Create the graph node responsible for executing requested tools.
- `self.graph.ainvoke`: Run an asynchronous graph or model operation with await.
- `loop.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `log_files.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.browser.close`: Release the resource; match setup with cleanup.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.playwright.stop`: Stop the receiver runtime/server/resource.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
