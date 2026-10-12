# Host, workers, registration, and shutdown

Source: [5_autogen/4_lab4_autogen_distributed.ipynb](../../../../../5_autogen/4_lab4_autogen_distributed.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Start gRPC host → start one or several workers → register agents → send judge request → collect replies → stop workers then host.

**Before source execution:** The source listens on localhost:50051. Practice uses a logical registry and opens no port. The all-in-one and separated-worker branches need independent integration checks.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Typed messages and agent identities

A runtime delivers a message to an identified handler. The handler owns its work. This dictionary-based simulation exposes type/key routing without starting AutoGen or network listeners.

**Ask yourself:** Will an unregistered type resolve?

**Practice:** Register a second handler and send the response to it. Explain which code routes and which code does the work.

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

## Follow the source in order

### Cell 1: Week 5 Day 4

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.tools.langchain`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.utilities`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ALL_IN_ONE_WORKER`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: Start with our Message class

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Define class `Message`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: And now - a host for our distributed runtime

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `host`. Re-executing this cell updates kernel state.
- `host.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Let's reintroduce a tool

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Store expression result in `serper`. Re-executing this cell updates kernel state.
- Store expression result in `langchain_serper`. Re-executing this cell updates kernel state.
- Store expression result in `autogen_serper`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `instruction1`. Re-executing this cell updates kernel state.
- Store expression result in `instruction2`. Re-executing this cell updates kernel state.
- Store expression result in `judge`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: And make some Agents

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Define class `Player1Agent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Player2Agent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Judge` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `self.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `worker.start`: Start the receiver runtime/server/resource.
- `Player1Agent.register`: Register an agent type, handler, or resource before use.
- `Player2Agent.register`: Register an agent type, handler, or resource before use.
- `Judge.register`: Register an agent type, handler, or resource before use.
- `worker1.start`: Start the receiver runtime/server/resource.
- `worker2.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- `worker.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `worker.stop`: Stop the receiver runtime/server/resource.
- `worker1.stop`: Stop the receiver runtime/server/resource.
- `worker2.stop`: Stop the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- `host.stop`: Stop the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `Message` | class |
| 11 | `Player1Agent` | class |
| 11 | `Player2Agent` | class |
| 11 | `Judge` | class |
| 11 | `__init__` | function |
| 11 | `handle_my_message_type` | async function |
| 11 | `__init__` | function |
| 11 | `handle_my_message_type` | async function |
| 11 | `__init__` | function |
| 11 | `handle_my_message_type` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
