# humandb

Source: [4_langgraph/community_contributions/tochi/humandb.ipynb](../../../../../../../4_langgraph/community_contributions/tochi/humandb.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Messages → A tool schema describes; a dispatcher executes → Evaluate, give feedback, and bound revision → Build a real local LangGraph without a model → Conditional routing makes the next step explicit → A callback receives a message and history → Configuration fills named placeholders → Request, status, and parsed body

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

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

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: 🗣️ Humanize DB - Query your SQL Database with Natural Language

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `langchain.chat_models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.runnables`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.runnables.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `llm`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: Configure the database

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Store expression result in `url`. Re-executing this cell updates kernel state.
- Store expression result in `DB_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `file.write`: Write data to a file-like object; inspect the path and mode.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `db`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `toolkit`. Re-executing this cell updates kernel state.
- Store expression result in `tools`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Tools

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store expression result in `get_schema_tool`. Re-executing this cell updates kernel state.
- Store expression result in `run_query_tool`. Re-executing this cell updates kernel state.
- Store expression result in `list_tables_tool`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `get_schema_node`. Re-executing this cell updates kernel state.
- Store expression result in `run_query_node`. Re-executing this cell updates kernel state.
- `ToolNode`: Create the graph node responsible for executing requested tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Define `list_tables(state)`. Its body runs when called.
- `list_tables_tool.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Define `call_get_schema(state)`. Its body runs when called.
- `llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Store expression result in `generate_query_system_prompt`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Define `generate_query(state)`. Its body runs when called.
- `llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: Query Evaluator Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Store expression result in `validate_query_system_prompt`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Define `check_query(state)`. Its body runs when called.
- `llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Define `should_continue(state)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Store expression result in `builder`. Re-executing this cell updates kernel state.
- Store expression result in `agent`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `builder.add_node`: Register a state-processing function under a node name.
- `builder.add_edge`: Connect one node to the next in a fixed path.
- `builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `builder.compile`: Build an executable graph or compiled object; this is separate from running it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: code

- Define `chat_with_agent(message, history)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Define `display_tables()`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 11 | `list_tables` | function |
| 12 | `call_get_schema` | function |
| 14 | `generate_query` | function |
| 17 | `check_query` | function |
| 18 | `should_continue` | function |
| 21 | `chat_with_agent` | function |
| 22 | `display_tables` | function |
| 23 | `user` | function |
| 23 | `bot` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
