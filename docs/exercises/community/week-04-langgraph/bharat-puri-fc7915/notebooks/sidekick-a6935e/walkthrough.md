# sidekick

Source: [4_langgraph/community_contributions/bharat_puri/sidekick.ipynb](../../../../../../../4_langgraph/community_contributions/bharat_puri/sidekick.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Evaluate, give feedback, and bound revision → Build a real local LangGraph without a model → A callback receives a message and history → Parameterized SQL and isolated data → Configuration fills named placeholders → Retrieve evidence before answering

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

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

## Follow the source in order

### Cell 1: LangGraph Sidekick – Week 4 Final Assignment

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.chat_message_histories`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Define `search_docs(query, folder)`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define `run_python(expression)`. Its body runs when called.
- Store list in `tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `memory`. Re-executing this cell updates kernel state.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `remember_message(role, message)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `planner_prompt`. Re-executing this cell updates kernel state.
- Store expression result in `worker_prompt`. Re-executing this cell updates kernel state.
- Define `planner_node(state)`. Its body runs when called.
- Define `worker_node(state)`. Its body runs when called.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `GraphState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `compiled_graph`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Define `ask_clarifying_questions(user_input)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Define `run_sidekick(user_input)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `compiled_graph.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define `interface_fn(user_input)`. Its body runs when called.
- Store expression result in `iface`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `view_memory()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.close`: Release the resource; match setup with cleanup.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `msg_obj.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `msg_data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- `iface.launch`: Start the UI/server; defining the UI alone does not start it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 6 | `search_docs` | function |
| 7 | `run_python` | function |
| 8 | `remember_message` | function |
| 9 | `planner_node` | function |
| 9 | `worker_node` | function |
| 10 | `GraphState` | class |
| 11 | `ask_clarifying_questions` | function |
| 12 | `run_sidekick` | function |
| 13 | `interface_fn` | function |
| 14 | `view_memory` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
