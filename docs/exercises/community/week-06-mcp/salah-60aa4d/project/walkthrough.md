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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

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

## Module-by-module reading map

### [6_mcp/community_contributions/salah/devops-monitor-agent/gradio_app.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/gradio_app.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `src.devops_floor` — Local or third-party dependency; follow its definition/import before execution.
- `src.simple_database` — Local or third-party dependency; follow its definition/import before execution.
- `src.tracers` — Local or third-party dependency; follow its definition/import before execution.
- `threading` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `MonitorUI` | class |
| 121 | `create_ui` | function |
| 12 | `__init__` | function |
| 19 | `get_system_status` | function |
| 42 | `get_metrics_display` | function |
| 55 | `get_metrics_summary` | function |
| 71 | `run_test_cycle_async` | async function |
| 79 | `run_test_cycle` | function |
| 82 | `start_monitoring_async` | async function |
| 93 | `start_monitoring` | function |
| 102 | `stop_monitoring` | function |
| 110 | `cleanup` | function |
| 117 | `get_monitoring_status` | function |
| 96 | `run_async` | function |
| 186 | `refresh_all` | function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.devops_floor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.simple_database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `src.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MonitorUI`. Separate the class definition, constructing an instance, and calling its methods.
- Define `create_ui()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.monitoring_thread.start`: Start the receiver runtime/server/resource.
- `summary.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.floor.stop`: Stop the receiver runtime/server/resource.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/__init__.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/__init__.py)

**Read in this order:**

- Store expression result in `__version__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/alert_manager.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/alert_manager.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `.simple_database` — Local or third-party dependency; follow its definition/import before execution.
- `.tracers` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `AlertManager` | class |
| 13 | `__init__` | function |
| 20 | `create_agent` | async function |
| 42 | `send_email_for_critical_alerts` | async function |
| 73 | `run_alert_cycle` | async function |
| 89 | `run_with_mcp_server` | async function |
| 101 | `run_with_trace` | async function |
| 108 | `run` | async function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.simple_database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AlertManager`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `alert.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `a.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/config.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/config.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `get_env_int` | function |
| 14 | `get_env_float` | function |
| 21 | `get_env_str` | function |
| 25 | `Config` | class |
| 47 | `validate` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_env_int(key, default)`. Its body runs when called.
- Define `get_env_float(key, default)`. Its body runs when called.
- Define `get_env_str(key, default)`. Its body runs when called.
- Define class `Config`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `config`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `errors.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/devops_floor.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/devops_floor.py)

**Imports:**

- `.alert_manager` — Local or third-party dependency; follow its definition/import before execution.
- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.simple_database` — Local or third-party dependency; follow its definition/import before execution.
- `.system_monitor` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `DevOpsFloor` | class |
| 11 | `__init__` | function |
| 23 | `should_run_monitor` | function |
| 28 | `should_run_alerts` | function |
| 33 | `run_system_monitor` | async function |
| 41 | `run_alert_manager` | async function |
| 49 | `run_cycle` | async function |
| 60 | `run_continuous` | async function |
| 75 | `stop` | function |
| 78 | `run_single_cycle` | async function |
| 84 | `get_status` | function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.system_monitor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.alert_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.simple_database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `DevOpsFloor`. Separate the class definition, constructing an instance, and calling its methods.
- `tasks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `self.system_monitor.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.alert_manager.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/devops_server.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/devops_server.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.simple_database` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `fastmcp` — Local or third-party dependency; follow its definition/import before execution.
- `psutil` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `get_system_metrics` | function |
| 65 | `check_system_health` | function |
| 153 | `create_custom_alert` | function |
| 177 | `resolve_system_alert` | function |
| 194 | `get_system_alerts` | function |
| 212 | `get_metrics_history` | function |
| 231 | `get_dashboard_data` | function |
| 255 | `system_status` | function |

**Read in this order:**

- Make available: `psutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fastmcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.simple_database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `mcp`. Re-executing this cell updates kernel state.
- Define `get_system_metrics()`. Its body runs when called.
- Define `check_system_health()`. Its body runs when called.
- Define `create_custom_alert(title, message, severity)`. Its body runs when called.
- Define `resolve_system_alert(alert_id)`. Its body runs when called.
- Define `get_system_alerts(status)`. Its body runs when called.
- Define `get_metrics_history(metric_type, limit)`. Its body runs when called.
- Define `get_dashboard_data()`. Its body runs when called.
- Define `system_status()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `FastMCP`: Create a tool/resource server; registration and serving are separate steps.
- `mcp.tool`: Register a function as a tool using a decorator or wrapper.
- `mcp.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `status_lines.extend`: Add every item from an iterable to a list.
- `alerts_created.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `issues_found.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `current_status.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `status_lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `summary.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/email_agent.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/email_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `send_email` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `send_email(subject, html_body)`. Its body runs when called.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/simple_database.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/simple_database.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `sqlite3` — Local database storage.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `get_db_connection` | function |
| 14 | `init_database` | function |
| 57 | `write_system_metric` | function |
| 69 | `get_recent_metrics` | function |
| 95 | `create_alert` | function |
| 116 | `resolve_alert` | function |
| 128 | `get_active_alerts` | function |
| 151 | `update_agent_status` | function |
| 172 | `get_agent_status` | function |
| 203 | `get_system_summary` | function |
| 227 | `cleanup_old_data` | function |

**Read in this order:**

- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_db_connection()`. Its body runs when called.
- Define `init_database()`. Its body runs when called.
- Define `write_system_metric(metric_type, value, status, metadata)`. Its body runs when called.
- Define `get_recent_metrics(limit, metric_type)`. Its body runs when called.
- Define `create_alert(alert_id, title, message, severity, metadata)`. Its body runs when called.
- Define `resolve_alert(alert_id)`. Its body runs when called.
- Define `get_active_alerts()`. Its body runs when called.
- Define `update_agent_status(agent_name, status, error_message)`. Its body runs when called.
- Define `get_agent_status(agent_name)`. Its body runs when called.
- Define `get_system_summary()`. Its body runs when called.
- Define `cleanup_old_data(days)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `conn.close`: Release the resource; match setup with cleanup.
- `params.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/system_monitor.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/system_monitor.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.simple_database` — Local or third-party dependency; follow its definition/import before execution.
- `.tracers` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `agents.mcp` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `contextlib` — Resource lifetime helpers.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `SystemMonitor` | class |
| 11 | `__init__` | function |
| 18 | `create_agent` | async function |
| 38 | `run_monitoring_cycle` | async function |
| 50 | `run_with_mcp_server` | async function |
| 62 | `run_with_trace` | async function |
| 69 | `run` | async function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.simple_database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tracers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SystemMonitor`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `AsyncExitStack`: Manage a variable number of asynchronous resource lifetimes.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `stack.enter_async_context`: Keep another async resource open until the surrounding stack closes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [6_mcp/community_contributions/salah/devops-monitor-agent/src/tracers.py](../../../../../../6_mcp/community_contributions/salah/devops-monitor-agent/src/tracers.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `secrets` — Local or third-party dependency; follow its definition/import before execution.
- `string` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `make_trace_id` | function |
| 28 | `SimpleTracer` | class |
| 41 | `DevOpsLogTracer` | class |
| 70 | `register_tracer` | function |
| 80 | `trace` | function |
| 29 | `__init__` | function |
| 34 | `__enter__` | function |
| 37 | `__exit__` | function |
| 42 | `shutdown` | function |
| 45 | `get_agent_name` | function |
| 57 | `on_trace_start` | function |
| 60 | `on_trace_end` | function |
| 63 | `on_span_start` | function |
| 66 | `on_span_end` | function |

**Read in this order:**

- Make available: `secrets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `string`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Define `make_trace_id(agent_name)`. Its body runs when called.
- Define class `SimpleTracer`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `DevOpsLogTracer`. Separate the class definition, constructing an instance, and calling its methods.
- Define `register_tracer()`. Its body runs when called.
- Define `trace(name, trace_id)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
