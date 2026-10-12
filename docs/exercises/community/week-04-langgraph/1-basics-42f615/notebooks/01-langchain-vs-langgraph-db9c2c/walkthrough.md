# 01 langchain vs langgraph

Source: [4_langgraph/community_contributions/1_basics/01_langchain_vs_langgraph.ipynb](../../../../../../../4_langgraph/community_contributions/1_basics/01_langchain_vs_langgraph.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Messages → A tool schema describes; a dispatcher executes → Build a real local LangGraph without a model → Conditional routing makes the next step explicit

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

### Build a real local LangGraph without a model

A graph is state plus nodes plus edges. Nodes return state updates. Compilation builds the executable workflow; invoke runs it. This exercises the installed LangGraph package locally.

**Ask yourself:** Which value will the uppercase node return?

**Practice:** Add a second node that appends an exclamation mark. Wire it between uppercase and END.

### Conditional routing makes the next step explicit

A route is a function of state. A worker may need tools, evaluation, another attempt, or user clarification. This exercise checks all branch labels without calling any model.

**Ask yourself:** Which route wins when a tool call is present?

**Practice:** Add an attempts field and route to END after a fixed limit. Explain why asking for clarification is a valid stop condition.

## Follow the source in order

### Cell 1: ⚖️ Quick Decision Guide: LangChain vs LangGraph

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: 💻 Code Examples

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.output_parsers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: 💬 Example 1: Simple Q&A System

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: 🔗 LangChain Version (Best for this use case)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.
- Store expression result in `output_parser`. Re-executing this cell updates kernel state.
- Store expression result in `chain`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- `chain.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: 📈 LangGraph Version (Overkill for this, but here's how it looks)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `ask_llm(state)`. Its body runs when called.
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

### Cell 10: 🎯 Example 2: Customer Support Agent with Decision Making

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: 🔗 LangChain Version (Gets messy)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Define `check_order_status(order_id)`. Its body runs when called.
- Define `process_refund(order_id)`. Its body runs when called.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: 📈 LangGraph Version (Much better for this!)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Define `process_refund(order_id)`. Its body runs when called.
- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `classify_intent(state)`. Its body runs when called.
- Define `should_clarify(state)`. Its body runs when called.
- Define `ask_clarification(state)`. Its body runs when called.
- Define `execute_action(state)`. Its body runs when called.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `graph_image`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph.add_node`: Register a state-processing function under a node name.
- `graph.add_edge`: Connect one node to the next in a fixed path.
- `graph.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph.compile`: Build an executable graph or compiled object; this is separate from running it.
- `app.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `state.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: 🔬 Example 3: Multi-Agent Research System

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: 📈 LangGraph Version (Perfect use case)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Define class `ResearchState` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Define `web_researcher(state)`. Its body runs when called.
- Define `academic_researcher(state)`. Its body runs when called.
- Define `synthesizer(state)`. Its body runs when called.
- Define `quality_check(state)`. Its body runs when called.
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

### Cell 19: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 9 | `State` | class |
| 9 | `ask_llm` | function |
| 12 | `check_order_status` | function |
| 12 | `process_refund` | function |
| 14 | `process_refund` | function |
| 14 | `State` | class |
| 14 | `classify_intent` | function |
| 14 | `should_clarify` | function |
| 14 | `ask_clarification` | function |
| 14 | `execute_action` | function |
| 18 | `ResearchState` | class |
| 18 | `web_researcher` | function |
| 18 | `academic_researcher` | function |
| 18 | `synthesizer` | function |
| 18 | `quality_check` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
