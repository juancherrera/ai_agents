# weather mcp server: project walkthrough

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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Register a real local FastMCP tool without starting a server

The decorator registers a Python function with FastMCP. Tool listing here happens directly in process; it neither launches a subprocess nor calls a hosted model.

**Ask yourself:** Which tool name will be discovered?

**Practice:** Register a second harmless function. To expose stdio later, use a separate script with server.run(transport="stdio"); stdout then carries protocol messages.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [6_mcp/community_contributions/weather_mcp_server/README.md](../../../../../../6_mcp/community_contributions/weather_mcp_server/README.md): Weather MCP Server. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [6_mcp/community_contributions/weather_mcp_server/push_server.py](../../../../../../6_mcp/community_contributions/weather_mcp_server/push_server.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `logging` — Application diagnostics.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `PushModelArgs` | class |
| 30 | `push` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define class `PushModelArgs` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `push(args)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `Field`: Describe a schema field or constrain allowed values.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/weather_mcp_server/weather_researcher.py](../../../../../../6_mcp/community_contributions/weather_mcp_server/weather_researcher.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `_make_request` | function |
| 44 | `get_current_weather` | function |
| 104 | `get_weather_forecast` | function |
| 179 | `get_marine_weather` | function |
| 255 | `get_comprehensive_weather` | function |
| 342 | `geocode_location` | function |
| 404 | `get_coordinates` | function |
| 460 | `_validate_coordinates` | function |

**Read in this order:**

- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `STORMGLASS_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `STORMGLASS_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `STORMGLASS_AVAILABLE`. Re-executing this cell updates kernel state.
- Define `_make_request(endpoint, params)`. Its body runs when called.
- Define `get_current_weather(latitude, longitude, location_name, country_code)`. Its body runs when called.
- Define `get_weather_forecast(latitude, longitude, location_name, country_code, hours)`. Its body runs when called.
- Define `get_marine_weather(latitude, longitude, location_name, country_code, hours)`. Its body runs when called.
- Define `get_comprehensive_weather(latitude, longitude, location_name, country_code, hours)`. Its body runs when called.
- Define `geocode_location(location_name, country_code)`. Its body runs when called.
- Define `get_coordinates(location_name, latitude, longitude, country_code)`. Its body runs when called.
- Define `_validate_coordinates(latitude, longitude)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `coords.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `geocode_result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `current_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `current_data.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `hour.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `hour.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/weather_mcp_server/weather_server.py](../../../../../../6_mcp/community_contributions/weather_mcp_server/weather_server.py)

**Imports:**

- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `mcp.server.fastmcp` — Tool/resource protocol, clients, and servers.
- `pathlib` — Explicit filesystem paths.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.
- `weather_researcher` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `_handle_error` | function |
| 60 | `_validate_coordinates` | function |
| 70 | `get_current_weather_tool` | async function |
| 120 | `get_weather_forecast_tool` | async function |
| 179 | `get_marine_weather_tool` | async function |
| 239 | `get_comprehensive_weather_tool` | async function |
| 299 | `geocode_location_tool` | async function |
| 335 | `read_api_status_resource` | async function |
| 362 | `read_documentation_resource` | async function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mcp.server.fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `current_dir`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `_handle_error(error, tool_name, context)`. Its body runs when called.
- Define `_validate_coordinates(latitude, longitude)`. Its body runs when called.
- Define `get_current_weather_tool(location_name, latitude, longitude, country_code)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_weather_forecast_tool(location_name, latitude, longitude, country_code, hours)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_marine_weather_tool(location_name, latitude, longitude, country_code, hours)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `get_comprehensive_weather_tool(location_name, latitude, longitude, country_code, hours)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `geocode_location_tool(location_name, country_code)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_api_status_resource()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `read_documentation_resource()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `json.dumps`: Convert Python data to JSON text.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
