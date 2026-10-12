# Images, schemas, adapters, teams, and MCP

Source: [5_autogen/2_lab2_autogen_agentchat.ipynb](../../../../../5_autogen/2_lab2_autogen_agentchat.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Build image/text input → request a typed image description → adapt LangChain tools → alternate researcher/evaluator turns → discover MCP fetch tools.

**Before source execution:** The source downloads an image, searches, writes a flights file, and starts an MCP subprocess. Dates and Windows compatibility notes are historical; current integration behavior is unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Text and image inputs are different content items

A multimodal message can contain text plus an image. A structured description can constrain fields such as orientation. This local example constructs a tiny image and validates a description without a model.

**Ask yourself:** Which orientation is valid for an image wider than it is tall?

**Practice:** Create a square image and update orientation. Explain that valid orientation syntax alone does not establish what a model saw.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Team turns need a termination condition

A round-robin team alternates speakers. A marker can end the conversation; a maximum turn count stops it even if the marker never appears.

**Ask yourself:** At what turn does this conversation stop?

**Practice:** Remove APPROVE and add a finite fallback when no approval occurs. Discuss accidental marker mentions.

### MCP tools, resources, and server lifetime

A tool performs an operation; a resource supplies readable context. A client discovers tool descriptions and calls them while the server is connected. This fixture is not a protocol transport.

**Ask yourself:** Is listing tool names the same as executing them?

**Practice:** Add a read-only dictionary resource and a tool returning a fixed date. Explain how discovery differs from invocation.

## Follow the source in order

### Cell 1: AutoGen AgentChat - Going deeper..

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `io`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `PIL`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: A multi-modal conversation

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Store expression result in `url`. Re-executing this cell updates kernel state.
- Store expression result in `pil_image`. Re-executing this cell updates kernel state.
- Store expression result in `img`. Re-executing this cell updates kernel state.
- `Image.open`: Open a file or resource. Inspect the mode and path before running it.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `multi_modal_message`. Re-executing this cell updates kernel state.
- `MultiModalMessage`: Construct a message containing text plus other media.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `model_client`. Re-executing this cell updates kernel state.
- Store expression result in `describer`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `reply`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Structured Outputs!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Define class `ImageDescription` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `model_client`. Re-executing this cell updates kernel state.
- Store expression result in `describer`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `reply`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Make available: `textwrap`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: Using LangChain tools from AutoGen

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_serper`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.
- Store list in `autogen_tools`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_file_management_tools`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `model_client`. Re-executing this cell updates kernel state.
- Store expression result in `agent`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `display`: Render a value inside a notebook.
- `autogen_tools.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `TextMessage`: Construct a typed text message with its source.
- `display`: Render a value inside a notebook.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: Team interactions

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 15: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.conditions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.teams`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_serper`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.
- Store expression result in `model_client`. Re-executing this cell updates kernel state.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.
- Store expression result in `primary_agent`. Re-executing this cell updates kernel state.
- Store expression result in `evaluation_agent`. Re-executing this cell updates kernel state.
- Store expression result in `text_termination`. Re-executing this cell updates kernel state.
- Store expression result in `team`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMentionTermination`: Stop when a selected text marker appears.
- `RoundRobinGroupChat`: Configure alternating agent turns and termination rules.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `team.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: Drumroll..

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: But wait - a not-so-small problem for Windows PC people

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 19: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.mcp`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `fetch_mcp_server`. Re-executing this cell updates kernel state.
- Store expression result in `fetcher`. Re-executing this cell updates kernel state.
- Store expression result in `model_client`. Re-executing this cell updates kernel state.
- Store expression result in `agent`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `display`: Render a value inside a notebook.
- `agent.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 8 | `ImageDescription` | class |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
