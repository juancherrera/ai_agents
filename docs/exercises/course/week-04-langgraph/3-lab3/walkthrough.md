# Async browser tools and graph execution

Source: [4_langgraph/3_lab3.ipynb](../../../../../4_langgraph/3_lab3.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Create browser/toolkit → inspect tools → navigate/extract → combine tools → compile checkpointed graph → await ainvoke.

**Before source execution:** The source contains a Windows workaround editing installed kernel code. Practice does not alter dependencies. Prefer the course module/script approach when notebook event-loop behavior fails.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Build a real local LangGraph without a model

A graph is state plus nodes plus edges. Nodes return state updates. Compilation builds the executable workflow; invoke runs it. This exercises the installed LangGraph package locally.

**Ask yourself:** Which value will the uppercase node return?

**Practice:** Add a second node that appends an exclamation mark. Wire it between uppercase and END.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

## Follow the source in order

### Cell 1: Welcome to Week 4, Day 4

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing_extensions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.graph.message`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.prebuilt`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langgraph.checkpoint.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: Asynchronous LangGraph

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: code

- Define class `State` using `TypedDict`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `graph_builder`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Define `push(text)`. Its body runs when called.
- Store expression result in `tool_push`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Extra installation step - if you don't have Node and Playwright on your computer

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: And now - after Installing Playwright, a heads up for Windows PC Users:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Make available: `nest_asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: The LangChain community

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.tools.playwright.utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `async_browser`. Re-executing this cell updates kernel state.
- Store expression result in `toolkit`. Re-executing this cell updates kernel state.
- Store expression result in `tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Store expression result in `tool_dict`. Re-executing this cell updates kernel state.
- Store expression result in `navigate_tool`. Re-executing this cell updates kernel state.
- Store expression result in `extract_text_tool`. Re-executing this cell updates kernel state.
- Store expression result in `text`. Re-executing this cell updates kernel state.
- `tool_dict.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Make available: `textwrap`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Store expression result in `all_tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `llm_with_tools`. Re-executing this cell updates kernel state.
- Define `chatbot(state)`. Its body runs when called.
- `llm.bind_tools`: Advertise tools to the model; a dispatcher still executes calls.
- `llm_with_tools.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Store expression result in `graph_builder`. Re-executing this cell updates kernel state.
- Store expression result in `memory`. Re-executing this cell updates kernel state.
- Store expression result in `graph`. Re-executing this cell updates kernel state.
- `StateGraph`: Start a graph builder using the declared state schema.
- `graph_builder.add_node`: Register a state-processing function under a node name.
- `graph_builder.add_conditional_edges`: Choose the next path from state using a routing function.
- `graph_builder.add_edge`: Connect one node to the next in a fixed path.
- `MemorySaver`: Keep graph checkpoints in memory for this process lifetime.
- `graph_builder.compile`: Build an executable graph or compiled object; this is separate from running it.
- `display`: Render a value inside a notebook.
- `ToolNode`: Create the graph node responsible for executing requested tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Store dictionary in `config`. Re-executing this cell updates kernel state.
- Define `chat(user_input, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `gr.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `graph.ainvoke`: Run an asynchronous graph or model operation with await.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 5 | `State` | class |
| 6 | `push` | function |
| 16 | `chatbot` | function |
| 18 | `chat` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
