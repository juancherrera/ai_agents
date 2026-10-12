# 02 langgraph fundamentals

Source: [4_langgraph/community_contributions/1_basics/02_langgraph_fundamentals.ipynb](../../../../../../../4_langgraph/community_contributions/1_basics/02_langgraph_fundamentals.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Evaluate, give feedback, and bound revision → Build a real local LangGraph without a model → Reducer vs. checkpoints vs. conversation identity → Conditional routing makes the next step explicit → A callback receives a message and history → Parameterized SQL and isolated data

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

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

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

## Follow the source in order

### Cell 1: LangGraph Fundamentals

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `operator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.sqlite`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: 🗺️ State, Nodes, and Edges

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `city_a(state)`. Its body runs when called.
- Define `city_b(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: 🔀 Reducer

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `node_a(state)`. Its body runs when called.
- Define `node_b(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define class `AgentState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `analyzer_agent(state)`. Its body runs when called.
- Define `responder_agent(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: 🛠️ Tools

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Define `send_notification(message)`. Its body runs when called.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `llm_with_tools`. Re-executing this cell updates kernel state.
- Define `chatbot(state)`. Its body runs when called.
- Store expression result in `tool_node`. Re-executing this cell updates kernel state.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `ToolNode`: Create the graph node responsible for executing requested tools.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.
- `llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: 💾 Memory

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Define `chatbot(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `conn`. Re-executing this cell updates kernel state.
- Store expression result in `memory`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Define `chat(message, _)`. Its body runs when called.
- Store expression result in `demo`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `SqliteSaver`: Use SQLite for graph checkpoints; inspect the database destination.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: 🚦 Routing Functions

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `search_page(state)`. Its body runs when called.
- Define `give_answer(state)`. Its body runs when called.
- Define `check_if_enough(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: ⚡ Superstep

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 15: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `step_1(state)`. Its body runs when called.
- Define `step_2(state)`. Its body runs when called.
- Define `step_3(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `display`: Render a value inside a notebook.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `State` | class |
| 4 | `city_a` | function |
| 4 | `city_b` | function |
| 6 | `State` | class |
| 6 | `node_a` | function |
| 6 | `node_b` | function |
| 7 | `AgentState` | class |
| 7 | `analyzer_agent` | function |
| 7 | `responder_agent` | function |
| 9 | `send_notification` | function |
| 9 | `State` | class |
| 9 | `chatbot` | function |
| 11 | `State` | class |
| 11 | `chatbot` | function |
| 11 | `chat` | function |
| 13 | `State` | class |
| 13 | `search_page` | function |
| 13 | `give_answer` | function |
| 13 | `check_if_enough` | function |
| 15 | `State` | class |
| 15 | `step_1` | function |
| 15 | `step_2` | function |
| 15 | `step_3` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
