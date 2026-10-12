# 4 lab4 autogen distributed

Source: [5_autogen/community_contributions/kisali/4_lab4_autogen_distributed.ipynb](../../../../../../../5_autogen/community_contributions/kisali/4_lab4_autogen_distributed.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Typed messages and agent identities → Host and workers have different responsibilities

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Typed messages and agent identities

A runtime delivers a message to an identified handler. The handler owns its work. This dictionary-based simulation exposes type/key routing without starting AutoGen or network listeners.

**Ask yourself:** Will an unregistered type resolve?

**Practice:** Register a second handler and send the response to it. Explain which code routes and which code does the work.

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

## Follow the source in order

### Cell 1: AutoGen Core - Distributed Agent Runtime

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Store expression result in `ALL_IN_ONE_WORKER`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: Simple Message class

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: code

- Define class `Message`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: Starting up the host service

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 7: code

- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `host`. Re-executing this cell updates kernel state.
- `host.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Introducing internet search tool

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_serper`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `favouring_instruction`. Re-executing this cell updates kernel state.
- Store expression result in `opposing_instruction`. Re-executing this cell updates kernel state.
- Store expression result in `judge_instruction`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: Creating Agents

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Define class `PlayerOneAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PlayerTwoAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `JudgeAgent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `self.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: Now we can set up the worker agent runtimes. We use GrpcWorkerAgentRuntime.

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `worker.start`: Start the receiver runtime/server/resource.
- `PlayerOneAgent.register`: Register an agent type, handler, or resource before use.
- `PlayerTwoAgent.register`: Register an agent type, handler, or resource before use.
- `JudgeAgent.register`: Register an agent type, handler, or resource before use.
- `worker1.start`: Start the receiver runtime/server/resource.
- `worker2.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- `worker.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: To stop the worker runtimes, we can call stop()

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `worker.stop`: Stop the receiver runtime/server/resource.
- `worker1.stop`: Stop the receiver runtime/server/resource.
- `worker2.stop`: Stop the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: We can call stop() to stop the host service.

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 20: code

- `host.stop`: Stop the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 5 | `Message` | class |
| 12 | `PlayerOneAgent` | class |
| 12 | `PlayerTwoAgent` | class |
| 12 | `JudgeAgent` | class |
| 12 | `__init__` | function |
| 12 | `handle_my_message_type` | async function |
| 12 | `__init__` | function |
| 12 | `handle_my_message_type` | async function |
| 12 | `__init__` | function |
| 12 | `handle_my_message_type` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
