# Typed messages in a standalone runtime

Source: [5_autogen/3_lab3_autogen_core.ipynb](../../../../../5_autogen/3_lab3_autogen_core.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Define Message → define RoutedAgent handler → register type → start runtime → send to AgentId → stop/close → compose multiple message handlers.

**Before source execution:** The first handler needs no model, while later handlers delegate to hosted/local clients. Registration, agent identity, message handling, and model behavior are distinct responsibilities.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Typed messages and agent identities

A runtime delivers a message to an identified handler. The handler owns its work. This dictionary-based simulation exposes type/key routing without starting AutoGen or network listeners.

**Ask yourself:** Will an unregistered type resolve?

**Practice:** Register a second handler and send the response to it. Explain which code routes and which code does the work.

### Run a real standalone AutoGen handler without a model

A local runtime can route ordinary Python messages without AgentChat or an LLM. Register the type, start the runtime, send to its type/key identity, then stop and close.

**Ask yourself:** Which handler receives the message sent to simple_agent/default?

**Practice:** Change the reply text and send another message. Explain why registration must precede delivery and why close is still necessary.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Team turns need a termination condition

A round-robin team alternates speakers. A marker can end the conversation; a maximum turn count stops it even if the marker never appears.

**Ask yourself:** At what turn does this conversation stop?

**Practice:** Remove APPROVE and add a finite fallback when no approval occurs. Discuss accidental marker mentions.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

## Follow the source in order

### Cell 1: And now - Week 3 Day 3

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: First we define our Message object

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Define class `Message`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: Now we define our Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Define class `SimpleAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: OK let's create a Standalone runtime and register our agent type

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `SimpleAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: Alright! Let's start a runtime and send a message

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 10: code

- `runtime.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store expression result in `agent_id`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: OK Now let's do something more interesting

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Define class `MyLLMAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `SimpleAgent.register`: Register an agent type, handler, or resource before use.
- `MyLLMAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `runtime.start`: Start the receiver runtime/server/resource.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: OK now let's show this at work - let's have 3 agents interact!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 19: code

- Make available: `autogen_ext.models.ollama`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Player1Agent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Player2Agent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- Store expression result in `JUDGE`. Re-executing this cell updates kernel state.
- Define class `RockPaperScissorsAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `self.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: code

- Store expression result in `runtime`. Re-executing this cell updates kernel state.
- `runtime.start`: Start the receiver runtime/server/resource.
- `Player1Agent.register`: Register an agent type, handler, or resource before use.
- `Player2Agent.register`: Register an agent type, handler, or resource before use.
- `RockPaperScissorsAgent.register`: Register an agent type, handler, or resource before use.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Store expression result in `agent_id`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `runtime.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- `runtime.stop`: Stop the receiver runtime/server/resource.
- `runtime.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `Message` | class |
| 6 | `SimpleAgent` | class |
| 6 | `__init__` | function |
| 6 | `on_my_message` | async function |
| 14 | `MyLLMAgent` | class |
| 14 | `__init__` | function |
| 14 | `handle_my_message_type` | async function |
| 19 | `Player1Agent` | class |
| 19 | `Player2Agent` | class |
| 19 | `__init__` | function |
| 19 | `handle_my_message_type` | async function |
| 19 | `__init__` | function |
| 19 | `handle_my_message_type` | async function |
| 20 | `RockPaperScissorsAgent` | class |
| 20 | `__init__` | function |
| 20 | `handle_my_message_type` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
