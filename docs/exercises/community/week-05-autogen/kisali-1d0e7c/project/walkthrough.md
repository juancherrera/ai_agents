# kisali: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

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

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

## Module-by-module reading map

### [5_autogen/community_contributions/kisali/agent.py](../../../../../../5_autogen/community_contributions/kisali/agent.py)

**Imports:**

- `autogen_agentchat.agents` — Conversational assistants and teams.
- `autogen_agentchat.messages` — Conversational assistants and teams.
- `autogen_core` — Typed messages and runtimes.
- `autogen_ext.models.openai` — Model/tool/runtime integrations for AutoGen.
- `messages` — Local or third-party dependency; follow its definition/import before execution.
- `random` — Demo/simulation randomness.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `Agent` | class |
| 22 | `__init__` | function |
| 32 | `handle_message` | async function |

**Read in this order:**

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Agent` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `TextMessage`: Construct a typed text message with its source.
- `self.send_message`: Deliver a message to an agent identity through a runtime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [5_autogen/community_contributions/kisali/creator.py](../../../../../../5_autogen/community_contributions/kisali/creator.py)

**Imports:**

- `autogen_agentchat.agents` — Conversational assistants and teams.
- `autogen_agentchat.messages` — Conversational assistants and teams.
- `autogen_core` — Typed messages and runtimes.
- `autogen_ext.models.openai` — Model/tool/runtime integrations for AutoGen.
- `dotenv` — Load explicit environment-file configuration.
- `importlib` — Local or third-party dependency; follow its definition/import before execution.
- `logging` — Application diagnostics.
- `messages` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `Creator` | class |
| 32 | `__init__` | function |
| 41 | `get_user_prompt` | function |
| 51 | `handle_my_message_type` | async function |

**Read in this order:**

- Make available: `messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `importlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `Creator` using `RoutedAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.
- `TextMessage`: Construct a typed text message with its source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `module.Agent.register`: Register an agent type, handler, or resource before use.
- `self.send_message`: Deliver a message to an agent identity through a runtime.
- `module.Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [5_autogen/community_contributions/kisali/messages.py](../../../../../../5_autogen/community_contributions/kisali/messages.py)

**Imports:**

- `autogen_core` — Typed messages and runtimes.
- `dataclasses` — Simple typed records.
- `glob` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `random` — Demo/simulation randomness.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `Message` | class |
| 11 | `find_recipient` | function |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `glob`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Message`. Separate the class definition, constructing an instance, and calling its methods.
- Define `find_recipient()`. Its body runs when called.
- `agent_names.remove`: Delete an item/file depending on the receiver. Check the target.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [5_autogen/community_contributions/kisali/world.py](../../../../../../5_autogen/community_contributions/kisali/world.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `autogen_core` — Typed messages and runtimes.
- `autogen_ext.runtimes.grpc` — Model/tool/runtime integrations for AutoGen.
- `creator` — Local or third-party dependency; follow its definition/import before execution.
- `messages` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `create_and_message` | async function |
| 18 | `main` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `creator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `autogen_ext.runtimes.grpc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `AGENTS`. Re-executing this cell updates kernel state.
- Define `create_and_message(worker, creator_id, i)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `host.start`: Start the receiver runtime/server/resource.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `worker.start`: Start the receiver runtime/server/resource.
- `Creator.register`: Register an agent type, handler, or resource before use.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `worker.send_message`: Deliver a message to an agent identity through a runtime.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `worker.stop`: Stop the receiver runtime/server/resource.
- `host.stop`: Stop the receiver runtime/server/resource.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
