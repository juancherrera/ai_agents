# random dog tutorial

Source: [6_mcp/community_contributions/random_dog_mcp_server_client/random_dog_tutorial.ipynb](../../../../../../../6_mcp/community_contributions/random_dog_mcp_server_client/random_dog_tutorial.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → MCP tools, resources, and server lifetime → Register a real local FastMCP tool without starting a server → Request, status, and parsed body

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

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

## Follow the source in order

### Cell 1: 🐕 Random Dog MCP Server & Client Tutorial

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: 🚀 Setup and Imports

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: 🔧 Understanding the MCP Server

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: 🧪 Testing the MCP Server Directly

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store dictionary in `params`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: 🐕 Getting Your First Random Dog

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.call_tool`: Invoke a tool with named arguments while its session is connected.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `dog_data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: 🖼️ Displaying the Dog Image

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 10: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.call_tool`: Invoke a tool with named arguments while its session is connected.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `dog_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: 🎯 Using the Custom Client

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Make available: `random_dog_client`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `client.list_tools`: Discover tool descriptions. Discovery does not invoke their functions.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: 🤖 AI Agent Integration

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `request`. Re-executing this cell updates kernel state.
- Store expression result in `model`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: 📋 Summary and Next Steps

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: 🔧 Utility Functions

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: code

- Define `get_simple_dog()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `simple_dog`. Re-executing this cell updates kernel state.
- Define `get_multiple_dogs(count)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `batch_dogs`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `MCPServerStdio`: Configure a subprocess-backed MCP connection and its lifetime.
- `server.call_tool`: Invoke a tool with named arguments while its session is connected.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `dog.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `dogs.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: 🔍 Troubleshooting

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 17 | `get_simple_dog` | async function |
| 17 | `get_multiple_dogs` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
