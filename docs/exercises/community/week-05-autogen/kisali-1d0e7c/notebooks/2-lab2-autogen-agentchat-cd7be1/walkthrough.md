# 2 lab2 autogen agentchat

Source: [5_autogen/community_contributions/kisali/2_lab2_autogen_agentchat.ipynb](../../../../../../../5_autogen/community_contributions/kisali/2_lab2_autogen_agentchat.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Await vs. concurrent work → Structured output validates shape → Evaluate, give feedback, and bound revision → Plan → search → synthesize → Team turns need a termination condition → Text and image inputs are different content items → Request, status, and parsed body

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

### Team turns need a termination condition

A round-robin team alternates speakers. A marker can end the conversation; a maximum turn count stops it even if the marker never appears.

**Ask yourself:** At what turn does this conversation stop?

**Practice:** Remove APPROVE and add a finite fallback when no approval occurs. Discuss accidental marker mentions.

### Text and image inputs are different content items

A multimodal message can contain text plus an image. A structured description can constrain fields such as orientation. This local example constructs a tiny image and validates a description without a model.

**Ask yourself:** Which orientation is valid for an image wider than it is tall?

**Practice:** Create a square image and update orientation. Explain that valid orientation syntax alone does not establish what a model saw.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: AutoGen Advanced

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `io`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `PIL`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: A multi-modal conversation

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: code

- Store expression result in `url`. Re-executing this cell updates kernel state.
- Store expression result in `pil_image`. Re-executing this cell updates kernel state.
- Store expression result in `img`. Re-executing this cell updates kernel state.
- `Image.open`: Open a file or resource. Inspect the mode and path before running it.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `multi_modal_message`. Re-executing this cell updates kernel state.
- `MultiModalMessage`: Construct a message containing text plus other media.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `image_describer`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Structured Outputs!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Define class `ImageDescription` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `image_describer`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `reply`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Make available: `textwrap`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: Using LangChain tools from AutoGen

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.agent_toolkits`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_serper_tool`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.
- Store list in `autogen_tools`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_file_management_tools`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `internet_search_agent`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `display`: Render a value inside a notebook.
- `autogen_tools.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `TextMessage`: Construct a typed text message with its source.
- `display`: Render a value inside a notebook.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: Team in AutoGen

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.conditions`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.teams`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `internet_search_tool`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.
- Store expression result in `prompt`. Re-executing this cell updates kernel state.
- Store expression result in `primary_agent`. Re-executing this cell updates kernel state.
- Store expression result in `evaluation_agent`. Re-executing this cell updates kernel state.
- Store expression result in `text_termination`. Re-executing this cell updates kernel state.
- Store expression result in `team`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMentionTermination`: Stop when a selected text marker appears.
- `RoundRobinGroupChat`: Configure alternating agent turns and termination rules.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Store expression result in `result`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `team.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: MCP Introdiction

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

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 9 | `ImageDescription` | class |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
