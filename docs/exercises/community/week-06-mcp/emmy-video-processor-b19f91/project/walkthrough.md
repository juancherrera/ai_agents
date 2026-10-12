# emmy video processor: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Register a real local FastMCP tool without starting a server

The decorator registers a Python function with FastMCP. Tool listing here happens directly in process; it neither launches a subprocess nor calls a hosted model.

**Ask yourself:** Which tool name will be discovered?

**Practice:** Register a second harmless function. To expose stdio later, use a separate script with server.run(transport="stdio"); stdout then carries protocol messages.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [6_mcp/community_contributions/emmy_video_processor/pyproject.toml](../../../../../../6_mcp/community_contributions/emmy_video_processor/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [6_mcp/community_contributions/emmy_video_processor/requirements.txt](../../../../../../6_mcp/community_contributions/emmy_video_processor/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [6_mcp/community_contributions/emmy_video_processor/gradio_app.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/gradio_app.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `subprocess` — Launch/manage another process.
- `video_processor.agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `handle_request` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `video_processor.agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `async_openai_client`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Define `handle_request(video_file, user_request, progress)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emmy_video_processor/mcp_server.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/mcp_server.py)

**Imports:**

- `video_processor` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `main` | function |

**Read in this order:**

- Make available: `video_processor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emmy_video_processor/video_processor/__init__.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/video_processor/__init__.py)

**Imports:**

- `.` — Local or third-party dependency; follow its definition/import before execution.
- `.mcp_tools` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.mcp_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emmy_video_processor/video_processor/agent.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/video_processor/agent.py)

**Imports:**

- `.mcp_tools` — Local or third-party dependency; follow its definition/import before execution.
- `__future__` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.exceptions` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.items` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `launch_mcp_server` | async function |
| 42 | `process_request` | async function |
| 84 | `_build_instructions` | function |
| 103 | `_format_video_state` | function |
| 120 | `_build_operations_log` | function |
| 130 | `_extract_file_outputs` | function |
| 152 | `_extract_raw_output` | function |
| 160 | `_gather_paths` | function |
| 195 | `_normalize_path_candidate` | function |
| 210 | `_try_parse_json` | function |

**Read in this order:**

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.exceptions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.items`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.mcp_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MAX_TURNS`. Re-executing this cell updates kernel state.
- Store expression result in `PROJECT_ROOT`. Re-executing this cell updates kernel state.
- Store expression result in `MCP_ENTRYPOINT`. Re-executing this cell updates kernel state.
- Define `launch_mcp_server()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `process_request(video_path, user_request, model)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `_build_instructions(video_path, video_info, user_request)`. Its body runs when called.
- Define `_format_video_state(video_path, video_info)`. Its body runs when called.
- Define `_build_operations_log(items)`. Its body runs when called.
- Define `_extract_file_outputs(items)`. Its body runs when called.
- Define `_extract_raw_output(raw_item)`. Its body runs when called.
- Define `_gather_paths(value)`. Its body runs when called.
- Define `_normalize_path_candidate(value)`. Its body runs when called.
- Define `_try_parse_json(value)`. Its body runs when called.
- Store list in `__all__`. Re-executing this cell updates kernel state.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `raw_item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `video_info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `stack.extend`: Add every item from an iterable to a list.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `log.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `paths.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `found.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `stack.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emmy_video_processor/video_processor/mcp_tools.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/video_processor/mcp_tools.py)

**Imports:**

- `.utils` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `fastmcp` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `get_video_metadata` | async function |
| 45 | `compress_video` | async function |
| 107 | `extract_audio` | async function |
| 163 | `convert_format` | async function |
| 220 | `resize_video` | async function |
| 273 | `extract_thumbnail` | async function |
| 315 | `trim_video` | async function |

**Read in this order:**

- Make available: `fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `get_video_metadata(video_path)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `compress_video(video_path, quality, target_size_mb)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `extract_audio(video_path, format, quality)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `convert_format(video_path, output_format)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `resize_video(video_path, resolution)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `extract_thumbnail(video_path, timestamp)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `trim_video(video_path, start_time, end_time, duration)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `cmd.extend`: Add every item from an iterable to a list.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `original_info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `new_info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `clip_info.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/emmy_video_processor/video_processor/utils.py](../../../../../../6_mcp/community_contributions/emmy_video_processor/video_processor/utils.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `subprocess` — Launch/manage another process.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `get_video_info` | function |
| 58 | `run_ffmpeg_command` | function |
| 79 | `create_temp_output_path` | function |
| 89 | `validate_video_file` | function |
| 109 | `format_duration` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_video_info(input_path)`. Its body runs when called.
- Define `run_ffmpeg_command(cmd, timeout)`. Its body runs when called.
- Define `create_temp_output_path(original_path, suffix, extension)`. Its body runs when called.
- Define `validate_video_file(file_path)`. Its body runs when called.
- Define `format_duration(seconds)`. Its body runs when called.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `video_stream.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `audio_stream.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
