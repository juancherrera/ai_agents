# 3 lab3 autogen core

Source: [5_autogen/community_contributions/kisali/3_lab3_autogen_core.ipynb](../../../../../../../5_autogen/community_contributions/kisali/3_lab3_autogen_core.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → Await vs. concurrent work → Typed messages and agent identities → Run a real standalone AutoGen handler without a model

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Typed messages and agent identities

A runtime delivers a message to an identified handler. The handler owns its work. This dictionary-based simulation exposes type/key routing without starting AutoGen or network listeners.

**Ask yourself:** Will an unregistered type resolve?

**Practice:** Register a second handler and send the response to it. Explain which code routes and which code does the work.

### Run a real standalone AutoGen handler without a model

A local runtime can route ordinary Python messages without AgentChat or an LLM. Register the type, start the runtime, send to its type/key identity, then stop and close.

**Ask yourself:** Which handler receives the message sent to simple_agent/default?

**Practice:** Change the reply text and send another message. Explain why registration must precede delivery and why close is still necessary.

## Follow the source in order

### Cell 1: AutoGen Core

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: Defining Message object

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: code

- Define class `Message`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: Defining our Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 7: code

- Define class `SimpleAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Creating a Standalone runtime and register our agent type

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `SimpleAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: Starting runtime and sending a message

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- `runtime.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Store expression result in `agent_id`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: Remember to always stop and close the runtime

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: Something more interesting!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Define class `LLMAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `SimpleAgent.register`: Register an agent type, handler, or resource before use.
- `LLMAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `runtime.start`: Start the receiver runtime/server/resource.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: Always remember to stop and close the runtime. This also applies if you want to re-run the cell above.

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 20: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: Introducing game player agents - having 3 agents interact!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 22: code

- Make available: `pyexpat`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `PlayerOneAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PlayerTwoAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- Store expression result in `JUDGE`. Re-executing this cell updates kernel state.
- Define class `RockPaperScissorsAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `self.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 24: code

- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `runtime.start`: Start the receiver runtime/server/resource.
- `PlayerOneAgent.register`: Register an agent type, handler, or resource before use.
- `PlayerTwoAgent.register`: Register an agent type, handler, or resource before use.
- `RockPaperScissorsAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: code

- Store expression result in `agent_id`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 26: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 5 | `Message` | class |
| 7 | `SimpleAgent` | class |
| 7 | `__init__` | function |
| 7 | `on_message` | async function |
| 16 | `LLMAgent` | class |
| 16 | `__init__` | function |
| 16 | `handle_message_type` | async function |
| 22 | `PlayerOneAgent` | class |
| 22 | `PlayerTwoAgent` | class |
| 22 | `__init__` | function |
| 22 | `handle_message_type` | async function |
| 22 | `__init__` | function |
| 22 | `handle_message_type` | async function |
| 23 | `RockPaperScissorsAgent` | class |
| 23 | `__init__` | function |
| 23 | `handle_message_type` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
