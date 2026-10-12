# sidekick research and action: project walkthrough

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

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

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

## Module-by-module reading map

### [4_langgraph/community_contributions/sidekick_research_and_action/app.py](../../../../../../4_langgraph/community_contributions/sidekick_research_and_action/app.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `sidekick` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `setup` | async function |
| 9 | `process_message` | async function |
| 17 | `reset` | async function |
| 22 | `free_resources` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sidekick`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `setup()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `process_message(sidekick, thread_id, message, success_criteria, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `reset()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `free_resources(sidekick)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/sidekick_research_and_action/markdown_pdf_tool.py](../../../../../../4_langgraph/community_contributions/sidekick_research_and_action/markdown_pdf_tool.py)

**Imports:**

- `langchain.agents` — Model/tool wrappers.
- `markdown` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `subprocess` — Launch/manage another process.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `markdown_to_pdf` | function |
| 136 | `get_markdown_pdf_tool` | function |

**Read in this order:**

- Make available: `markdown`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `markdown_to_pdf(filename)`. Its body runs when called.
- Define `get_markdown_pdf_tool()`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `temp_html.write`: Write data to a file-like object; inspect the path and mode.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.unlink`: Delete a filesystem path. Do not run it just to inspect a source example.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/sidekick_research_and_action/sidekick.py](../../../../../../4_langgraph/community_contributions/sidekick_research_and_action/sidekick.py)

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
- `pydantic` — Validated structured objects.
- `sidekick_tools` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `typing_extensions` — Additional typing features.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `State` | class |
| 29 | `CoordinationPlan` | class |
| 37 | `EvaluatorOutput` | class |
| 43 | `Sidekick` | class |
| 44 | `__init__` | function |
| 56 | `setup` | async function |
| 76 | `coordinator_agent` | function |
| 136 | `research_agent` | function |
| 200 | `action_agent` | function |
| 275 | `evaluator` | function |
| 320 | `coordinator_router` | function |
| 331 | `research_router` | function |
| 347 | `action_router` | function |
| 356 | `evaluator_router` | function |
| 363 | `build_graph` | async function |
| 388 | `run_superstep` | async function |
| 422 | `cleanup` | async function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing_extensions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sidekick_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CoordinationPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EvaluatorOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Sidekick`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Field`: Describe a schema field or constrain allowed values.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `base_llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `base_llm.with_structured_output`: Request a result matching the supplied schema.
- `self.coordinator_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.evaluator_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `task_plan.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `self.research_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `self.action_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `ToolNode`: Create the graph node responsible for executing requested tools.
- `self.graph.ainvoke`: Run an asynchronous graph or model operation with await.
- `state.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `agent_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `agent_status.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.browser.close`: Release the resource; match setup with cleanup.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.playwright.stop`: Stop the receiver runtime/server/resource.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/sidekick_research_and_action/sidekick_tools.py](../../../../../../4_langgraph/community_contributions/sidekick_research_and_action/sidekick_tools.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain.agents` — Model/tool wrappers.
- `langchain_community.agent_toolkits` — Search, browser, document, and file integrations.
- `langchain_community.tools.wikipedia.tool` — Search, browser, document, and file integrations.
- `langchain_community.utilities` — Search, browser, document, and file integrations.
- `langchain_community.utilities.wikipedia` — Search, browser, document, and file integrations.
- `langchain_experimental.tools` — Experimental tools including a Python REPL.
- `markdown_pdf_tool` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `playwright.async_api` — Browser automation with setup/action/cleanup.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `playwright_tools` | async function |
| 26 | `push` | function |
| 31 | `get_file_tools` | function |
| 35 | `get_research_tools` | async function |
| 54 | `get_action_tools` | async function |

**Read in this order:**

- Make available: `playwright.async_api`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools.wikipedia.tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_experimental.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities.wikipedia`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `markdown_pdf_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Define `playwright_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `push(text)`. Its body runs when called.
- Define `get_file_tools()`. Its body runs when called.
- Define `get_research_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_action_tools()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `async_playwright(...).start`: Start the receiver runtime/server/resource.
- `playwright.chromium.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
