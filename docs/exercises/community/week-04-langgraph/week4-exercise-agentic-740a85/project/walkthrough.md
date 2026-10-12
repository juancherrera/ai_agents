# week4 exercise agentic.py: project walkthrough

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Text and image inputs are different content items

A multimodal message can contain text plus an image. A structured description can constrain fields such as orientation. This local example constructs a tiny image and validates a description without a model.

**Ask yourself:** Which orientation is valid for an image wider than it is tall?

**Practice:** Create a square image and update orientation. Explain that valid orientation syntax alone does not establish what a model saw.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [4_langgraph/community_contributions/week4_exercise_agentic.py](../../../../../../4_langgraph/community_contributions/week4_exercise_agentic.py)

**Imports:**

- `IPython.display` — Notebook display/shell facilities.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `langchain.agents` — Model/tool wrappers.
- `langchain_community.tools` — Search, browser, document, and file integrations.
- `langchain_community.utilities` — Search, browser, document, and file integrations.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_core.prompts` — Message/tool interfaces.
- `langchain_experimental.tools` — Experimental tools including a Python REPL.
- `langchain_openai` — Compatible model integration for LangChain.
- `langgraph.checkpoint.memory` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `langgraph.graph.message` — State, nodes, routing, and checkpoints.
- `langgraph.prebuilt` — State, nodes, routing, and checkpoints.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.lib.pagesizes` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.lib.styles` — Local or third-party dependency; follow its definition/import before execution.
- `reportlab.platypus` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `typing_extensions` — Additional typing features.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 58 | `Plan` | class |
| 92 | `planner_node` | function |
| 134 | `SidekickState` | class |
| 141 | `worker_logic` | function |
| 202 | `Review` | class |
| 216 | `review_logic` | function |
| 242 | `SidekickState` | class |
| 278 | `push` | function |
| 289 | `run_consultant_sidekick` | async function |
| 332 | `clean_text` | function |
| 344 | `create_pdf` | function |
| 366 | `download_plan` | function |
| 370 | `download_output` | function |
| 374 | `download_review` | function |
| 380 | `safe_update` | function |
| 405 | `run_all` | async function |
| 431 | `save_temp_pdf` | function |

**Read in this order:**

- Make available: `langchain_experimental.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing_extensions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_experimental.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.lib.pagesizes`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.platypus`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `reportlab.lib.styles`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `BASE_URL`. Re-executing this cell updates kernel state.
- Define class `Plan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_prompt`. Re-executing this cell updates kernel state.
- Store expression result in `planner_llm`. Re-executing this cell updates kernel state.
- Define `planner_node(state)`. Its body runs when called.
- Store expression result in `wiki_wrapper`. Re-executing this cell updates kernel state.
- Store expression result in `search_tool`. Re-executing this cell updates kernel state.
- Store expression result in `wiki_tool`. Re-executing this cell updates kernel state.
- Store expression result in `python_tool`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Store expression result in `worker_llm`. Re-executing this cell updates kernel state.
- Store expression result in `worker_node`. Re-executing this cell updates kernel state.
- Define class `SidekickState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `worker_logic(state)`. Its body runs when called.
- Define class `Review` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `reviewer_llm`. Re-executing this cell updates kernel state.
- Define `review_logic(state)`. Its body runs when called.
- Define class `SidekickState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `planner_logic`. Re-executing this cell updates kernel state.
- Store expression result in `memory`. Re-executing this cell updates kernel state.
- Store expression result in `consultant_graph`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Define `push(text)`. Its body runs when called.
- Define `run_consultant_sidekick(user_input, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `clean_text(text)`. Its body runs when called.
- Define `create_pdf(title, content)`. Its body runs when called.
- Define `download_plan(plan)`. Its body runs when called.
- Define `download_output(output)`. Its body runs when called.
- Define `download_review(review)`. Its body runs when called.
- Define `safe_update(value, visible)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ToolNode`: Create the graph node responsible for executing requested tools.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.
- `Field`: Describe a schema field or constrain allowed values.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `planner_prompt.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `planner_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `reviewer_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `story.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `plan.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `consultant_graph.ainvoke`: Run an asynchronous graph or model operation with await.
- `review.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tmp.write`: Write data to a file-like object; inspect the path and mode.
- `tmp.close`: Release the resource; match setup with cleanup.
- `worker_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `buffer.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
