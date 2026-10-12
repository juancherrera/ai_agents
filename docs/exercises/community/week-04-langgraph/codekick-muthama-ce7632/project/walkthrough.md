# codekick muthama: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Build a real local LangGraph without a model

A graph is state plus nodes plus edges. Nodes return state updates. Compilation builds the executable workflow; invoke runs it. This exercises the installed LangGraph package locally.

**Ask yourself:** Which value will the uppercase node return?

**Practice:** Add a second node that appends an exclamation mark. Wire it between uppercase and END.

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

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Module-by-module reading map

### [4_langgraph/community_contributions/codekick_muthama/app.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/app.py)

**Imports:**

- `gradio` — Python callback-based web/chat interfaces.
- `graph` — Local or third-party dependency; follow its definition/import before execution.
- `orchestrator` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `run_agent` | function |
| 15 | `build_ui` | function |

**Read in this order:**

- Make available: `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `orchestrator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run_agent(user_prompt, recursion_limit)`. Its body runs when called.
- Define `build_ui()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.
- `resp.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/codekick_muthama/graph.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/graph.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain.globals` — Model/tool wrappers.
- `langchain_core.output_parsers` — Message/tool interfaces.
- `langchain_groq` — Local or third-party dependency; follow its definition/import before execution.
- `langgraph.constants` — State, nodes, routing, and checkpoints.
- `langgraph.graph` — State, nodes, routing, and checkpoints.
- `langgraph.prebuilt` — State, nodes, routing, and checkpoints.
- `prompts` — Local or third-party dependency; follow its definition/import before execution.
- `states` — Local or third-party dependency; follow its definition/import before execution.
- `tools` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 20 | `_extract_text_from_response` | function |
| 44 | `planner_agent` | function |
| 69 | `architect_agent` | function |
| 95 | `coder_agent` | function |

**Read in this order:**

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.globals`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_groq`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.constants`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.output_parsers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `states`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Define `_extract_text_from_response(resp)`. Its body runs when called.
- Define `planner_agent(state)`. Its body runs when called.
- Define `architect_agent(state)`. Its body runs when called.
- Define `coder_agent(state)`. Its body runs when called.
- Store expression result in `graph_builder`. Re-executing this cell updates kernel state.
- Store expression result in `agent`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph_builder.add_node`: Register a state-processing function under a node name.
- `graph_builder.add_edge`: Connect one node to the next in a fixed path.
- `graph_builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph_builder.compile`: Build an executable graph or compiled object; this is separate from running it.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `parser.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `read_file.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `react_agent.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `agent.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `s.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/codekick_muthama/orchestrator.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/orchestrator.py)

**Imports:**

- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `ast` — Local or third-party dependency; follow its definition/import before execution.
- `graph` — Local or third-party dependency; follow its definition/import before execution.
- `inspect` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `pydantic` — Validated structured objects.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `tools` — Local or third-party dependency; follow its definition/import before execution.
- `traceback` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `WriteFileArgs` | class |
| 21 | `ReadFileArgs` | class |
| 33 | `extract_json` | function |
| 50 | `is_safe_args` | function |
| 64 | `try_parse_json_like` | function |
| 105 | `validate_args_with_model` | function |
| 114 | `safe_call_tool` | function |
| 171 | `run_with_orchestration` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ast`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `inspect`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `WriteFileArgs` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReadFileArgs` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store dictionary in `TOOL_REGISTRY`. Re-executing this cell updates kernel state.
- Store expression result in `JSON_OBJ_RE`. Re-executing this cell updates kernel state.
- Define `extract_json(text)`. Its body runs when called.
- Define `is_safe_args(args)`. Its body runs when called.
- Define `try_parse_json_like(obj)`. Its body runs when called.
- Define `validate_args_with_model(arg_model, args)`. Its body runs when called.
- Define `safe_call_tool(tool_id, args)`. Its body runs when called.
- Define `run_with_orchestration(user_prompt, recursion_limit, agent_kwargs)`. Its body runs when called.
- `re.compile`: Build an executable graph or compiled object; this is separate from running it.
- `TOOL_REGISTRY.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `entry.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `json.dumps`: Convert Python data to JSON text.
- `parsed.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tool_result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `agent.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `call_args.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/codekick_muthama/prompts.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/prompts.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `planner_prompt` | function |
| 11 | `architect_prompt` | function |
| 31 | `coder_system_prompt` | function |

**Read in this order:**

- Define `planner_prompt(user_prompt)`. Its body runs when called.
- Define `architect_prompt(plan)`. Its body runs when called.
- Define `coder_system_prompt()`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/codekick_muthama/states.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/states.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `File` | class |
| 10 | `Plan` | class |
| 17 | `ImplementationTask` | class |
| 21 | `TaskPlan` | class |
| 25 | `CoderState` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `File` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Plan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ImplementationTask` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TaskPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CoderState` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [4_langgraph/community_contributions/codekick_muthama/tools.py](../../../../../../4_langgraph/community_contributions/codekick_muthama/tools.py)

**Imports:**

- `langchain_core.tools` — Message/tool interfaces.
- `pathlib` — Explicit filesystem paths.
- `subprocess` — Launch/manage another process.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `safe_path_for_project` | function |
| 18 | `write_file` | function |
| 28 | `read_file` | function |
| 38 | `get_current_directory` | function |
| 44 | `list_files` | function |
| 54 | `run_cmd` | function |
| 61 | `init_project_root` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `PROJECT_ROOT`. Re-executing this cell updates kernel state.
- Define `safe_path_for_project(path)`. Its body runs when called.
- Define `write_file(path, content)`. Its body runs when called.
- Define `read_file(path)`. Its body runs when called.
- Define `get_current_directory()`. Its body runs when called.
- Define `list_files(directory)`. Its body runs when called.
- Define `run_cmd(cmd, cwd, timeout)`. Its body runs when called.
- Define `init_project_root()`. Its body runs when called.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
