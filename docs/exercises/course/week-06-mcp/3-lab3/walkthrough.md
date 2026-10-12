# Memory, search, market APIs, and caching

Source: [6_mcp/3_lab3.ipynb](../../../../../6_mcp/3_lab3.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Connect memory server → store/recall entities → connect search → inspect market data → cache end-of-day prices → compare local servers and hosted services.

**Before source execution:** The source market module can return random simulated values after API failure. Cached data must not be described as verified current prices. Plan/pricing remarks are historical.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

## Follow the source in order

### Cell 1: Welcome to Week 6 Day 3!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: The first type of MCP Server: runs locally, everything local

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Store dictionary in `params`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `request`. Re-executing this cell updates kernel state.
- Store expression result in `model`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Check the trace:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: The 2nd type of MCP server - runs locally, calls a web service

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 10: code

- Store dictionary in `env`. Re-executing this cell updates kernel state.
- Store dictionary in `params`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store formatted string in `request`. Re-executing this cell updates kernel state.
- Store expression result in `model`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: As usual, check out the trace:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: And now the third type: running remotely

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 15: And back to the 2nd type: the Polygon.io MCP Server

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: PLEASE READ!!-

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: NEW SECTION: Introducing polygon.io

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Store expression result in `polygon_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Make available: `polygon`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- `RESTClient`: Construct the market-data client; retrieval is a separate API call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: Wrapped into a python module that caches end of day prices

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: code

- Make available: `market`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: And I've made this into an MCP Server

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 24: code

- Store dictionary in `params`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: Let's try it out!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 26: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `request`. Re-executing this cell updates kernel state.
- Store expression result in `model`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 27: Polygon.io Part 2: Paid Plan - Totally Optional!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 28: code

- Store dictionary in `params`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 29: Wow that's a lot of tools!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 30: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `request`. Re-executing this cell updates kernel state.
- Store expression result in `model`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 31: Setting up your .env file

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 32: code

- Store expression result in `polygon_plan`. Re-executing this cell updates kernel state.
- Store expression result in `is_paid_polygon`. Re-executing this cell updates kernel state.
- Store expression result in `is_realtime_polygon`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 33: And that's it for today!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 34: Exercises

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
