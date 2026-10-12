# 2 lab2 product recommendation

Source: [2_openai/community_contributions/2_lab2_product_recommendation.ipynb](../../../../../../../2_openai/community_contributions/2_lab2_product_recommendation.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Agent as a tool vs. handoff → Plan → search → synthesize → A callback receives a message and history

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

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

## Follow the source in order

### Cell 1: Creating a Product Recommendation Solution to help people with analysis paralysis

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `litellm`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `duckduckgo_search`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.extensions.models.litellm_model`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ipywidgets`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Define `test_api_keys()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `check_system_status()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `apis_working.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `language_selector`. Re-executing this cell updates kernel state.
- Define class `SessionManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `session`. Re-executing this cell updates kernel state.
- Define `on_language_change(change)`. Its body runs when called.
- Store expression result in `selected_language`. Re-executing this cell updates kernel state.
- `display`: Render a value inside a notebook.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.conversation_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store formatted string in `region_search_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `region_search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Let's start defining our specialist agents

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Store expression result in `gemini_model_config`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store formatted string in `electronics_smarttech_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `electronics_smarttech_agent`. Re-executing this cell updates kernel state.
- Store formatted string in `lifestyle_wellness_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `lifestyle_wellness_agent`. Re-executing this cell updates kernel state.
- Store formatted string in `entertainment_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `entertainment_agent`. Re-executing this cell updates kernel state.
- Store formatted string in `home_garden_pets_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `home_garden_pets_agent`. Re-executing this cell updates kernel state.
- Store formatted string in `fashion_lifestyle_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `fashion_lifestyle_agent`. Re-executing this cell updates kernel state.
- Store formatted string in `sports_auto_industry_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `sports_auto_industry_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `electronics_smarttech_tool`. Re-executing this cell updates kernel state.
- Store expression result in `lifestyle_wellness_tool`. Re-executing this cell updates kernel state.
- Store expression result in `entertainment_tool`. Re-executing this cell updates kernel state.
- Store expression result in `home_garden_pets_tool`. Re-executing this cell updates kernel state.
- Store expression result in `fashion_lifestyle_tool`. Re-executing this cell updates kernel state.
- Store expression result in `sports_auto_industry_tool`. Re-executing this cell updates kernel state.
- `electronics_smarttech_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `lifestyle_wellness_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `entertainment_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `home_garden_pets_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `fashion_lifestyle_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sports_auto_industry_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Define `web_search_tool(query)`. Its body runs when called.
- Define `create_search_tool()`. Its body runs when called.
- Store expression result in `web_search_tool_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `search_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `formatted_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Define `create_manager_instructions()`. Its body runs when called.
- Define `get_manager_agent()`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: Display Menu Options for User

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Define `translate_text(text, target_language)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `display_menu()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `quick_test()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `test_intelligent_region()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `debug_test()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `debug_instructions()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `options.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `chat_interface(message, history)`. Its body runs when called.
- Define `launch_chat()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `examples.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `test_api_keys` | async function |
| 4 | `check_system_status` | function |
| 5 | `SessionManager` | class |
| 5 | `on_language_change` | function |
| 5 | `__init__` | function |
| 5 | `add_message` | function |
| 5 | `get_context` | function |
| 5 | `get_context_string` | function |
| 11 | `web_search_tool` | function |
| 11 | `create_search_tool` | function |
| 11 | `search_function` | function |
| 11 | `custom_run` | async function |
| 12 | `create_manager_instructions` | function |
| 12 | `get_manager_agent` | function |
| 14 | `translate_text` | async function |
| 14 | `display_menu` | async function |
| 14 | `quick_test` | async function |
| 14 | `test_intelligent_region` | async function |
| 14 | `debug_test` | async function |
| 14 | `debug_instructions` | function |
| 15 | `chat_interface` | function |
| 15 | `launch_chat` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
