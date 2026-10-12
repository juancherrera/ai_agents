# philip: project walkthrough

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

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

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

## Module-by-module reading map

### [4_langgraph/community_contributions/philip/app.py](../../../../../../4_langgraph/community_contributions/philip/app.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `content_agent` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `setup` | async function |
| 17 | `process_request` | async function |
| 60 | `reset` | async function |
| 111 | `process_sync` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `content_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `setup()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `process_request(agent, message, success_criteria, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `reset()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `response_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `agent.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `seo_metadata.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/content_agent.py](../../../../../../4_langgraph/community_contributions/philip/content_agent.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `langgraph.checkpoint.memory` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `nodes` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 29 | `EvaluatorOutput` | class |
| 53 | `ContentAgent` | class |
| 56 | `__init__` | function |
| 63 | `evaluator` | function |
| 168 | `route_after_evaluation` | function |
| 209 | `route_by_content_type` | function |
| 224 | `build_graph` | async function |
| 272 | `run` | async function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `nodes`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EvaluatorOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ContentAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Field`: Describe a schema field or constrain allowed values.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `self.evaluator_llm.with_structured_output`: Request a result matching the supplied schema.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `state.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.evaluator_llm_with_output.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `evaluator_feedback.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph_builder.add_node`: Register a state-processing function under a node name.
- `graph_builder.add_edge`: Connect one node to the next in a fixed path.
- `graph_builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph_builder.compile`: Build an executable graph or compiled object; this is separate from running it.
- `self.graph.ainvoke`: Run an asynchronous graph or model operation with await.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/__init__.py](../../../../../../4_langgraph/community_contributions/philip/nodes/__init__.py)

**Imports:**

- `.blog_generator` — Local or third-party dependency; follow its definition/import before execution.
- `.refiner` — Local or third-party dependency; follow its definition/import before execution.
- `.research` — Local or third-party dependency; follow its definition/import before execution.
- `.router` — Local or third-party dependency; follow its definition/import before execution.
- `.seo_optimizer` — Local or third-party dependency; follow its definition/import before execution.
- `.social_generator` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.router`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.research`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.blog_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.social_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.seo_optimizer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.refiner`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/blog_generator.py](../../../../../../4_langgraph/community_contributions/philip/nodes/blog_generator.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `blog_generator_node` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `blog_generator_node(state)`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requirements.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `research_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `seo_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/refiner.py](../../../../../../4_langgraph/community_contributions/philip/nodes/refiner.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `content_refiner_node` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `content_refiner_node(state)`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `evaluator_feedback.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requirements.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `tasks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tasks.extend`: Add every item from an iterable to a list.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/research.py](../../../../../../4_langgraph/community_contributions/philip/nodes/research.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain_community.utilities` — Search, browser, document, and file integrations.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `research_node` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `research_node(state)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `state.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `serper.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/router.py](../../../../../../4_langgraph/community_contributions/philip/nodes/router.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `ContentTypeDecision` | class |
| 45 | `content_type_router` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ContentTypeDecision` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `content_type_router(state)`. Its body runs when called.
- `Field`: Describe a schema field or constrain allowed values.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.with_structured_output`: Request a result matching the supplied schema.
- `llm_with_structure.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/seo_optimizer.py](../../../../../../4_langgraph/community_contributions/philip/nodes/seo_optimizer.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `seo_optimizer_node` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `seo_optimizer_node(state)`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `requirements.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `seo_llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/nodes/social_generator.py](../../../../../../4_langgraph/community_contributions/philip/nodes/social_generator.py)

**Imports:**

- `langchain_core.messages` — Message/tool interfaces.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `state` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `social_generator_node` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `social_generator_node(state)`. Its body runs when called.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `platform_configs.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requirements.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `research_data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/philip/state.py](../../../../../../4_langgraph/community_contributions/philip/state.py)

**Imports:**

- `langgraph.graph.message` — State, nodes, routing, and checkpoints.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `ContentState` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ContentState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
