# autonomous trader with evaluator

Source: [6_mcp/community_contributions/kwabena/autonomous_trader with evaluator.ipynb](../../../../../../../6_mcp/community_contributions/kwabena/autonomous_trader%20with%20evaluator.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Await vs. concurrent work → Evaluate, give feedback, and bound revision → Agent as a tool vs. handoff → MCP tools, resources, and server lifetime → Cache and simulation are not live prices → Simulated accounts change state

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Simulated accounts change state

Buying shares updates cash, holdings, and a transaction record. Practice against a dictionary so you do not import account modules that create or alter databases.

**Ask yourself:** How much cash remains after buying three fictional shares at 100 each?

**Practice:** Try an unaffordable order and confirm the balance is unchanged. Explain why reset is a data-changing action.

## Follow the source in order

### Cell 1: Autonomous Trader with an Evaluator

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `project_root`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts_client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `polygon_plan`. Re-executing this cell updates kernel state.
- Store expression result in `is_paid_polygon`. Re-executing this cell updates kernel state.
- Store expression result in `is_realtime_polygon`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `trader_mcp_server_params`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Researcher, Trader and Evaluator MCP Servers

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Store dictionary in `brave_env`. Re-executing this cell updates kernel state.
- Store list in `researcher_mcp_server_params`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `evaluator_server_path`. Re-executing this cell updates kernel state.
- Store list in `evaluator_mcp_server_params`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `researcher_mcp_servers`. Re-executing this cell updates kernel state.
- Store expression result in `trader_mcp_servers`. Re-executing this cell updates kernel state.
- Store expression result in `evaluator_mcp_servers`. Re-executing this cell updates kernel state.
- Store expression result in `mcp_servers`. Re-executing this cell updates kernel state.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: Researcher Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Define `get_researcher(mcp_servers)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define `get_researcher_tool(mcp_servers)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `researcher.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Store expression result in `research_question`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `researcher`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Store expression result in `project_root`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `ed_initial_strategy`. Re-executing this cell updates kernel state.
- `Account.get(...).reset`: Reset state. For accounts, this can replace stored data.
- `display`: Render a value inside a notebook.
- `Account.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: Trader Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Store expression result in `agent_name`. Re-executing this cell updates kernel state.
- Store expression result in `account_details`. Re-executing this cell updates kernel state.
- Store expression result in `strategy`. Re-executing this cell updates kernel state.
- Store formatted string in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `researcher_tool`. Re-executing this cell updates kernel state.
- Store expression result in `trader`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: Evaluator Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: code

- Define `get_evaluator(evaluator_servers, trader_name)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `evaluator`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `server.connect`: Open a database or network/server connection; identify the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 12 | `get_researcher` | async function |
| 13 | `get_researcher_tool` | async function |
| 21 | `get_evaluator` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
