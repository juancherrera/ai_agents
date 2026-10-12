# 1 lab1 autogen agentchat

Source: [5_autogen/community_contributions/kisali/1_lab1_autogen_agentchat.ipynb](../../../../../../../5_autogen/community_contributions/kisali/1_lab1_autogen_agentchat.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Parameterized SQL and isolated data

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

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

## Follow the source in order

### Cell 1: AutoGen Introduction

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: Using Google's Gemini Models.

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Applying the message to model

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: Introducing the Agent using AutoGen AgentsChat

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 10: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `travel_agent`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: Putting everything together with on_messages

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `response`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: Let's make a local database of ticket prices. This will act as a long term memory where the agent can refer to.

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `connection`. Re-executing this cell updates kernel state.
- Store expression result in `conn`. Re-executing this cell updates kernel state.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `conn.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `connection.commit`: Persist pending database changes or another transaction.
- `connection.close`: Release the resource; match setup with cleanup.
- `os.remove`: Delete an item/file depending on the receiver. Check the target.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Define `save_city_price(city_name, round_trip_price)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `conn.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `connection.commit`: Persist pending database changes or another transaction.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `connection.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Define `get_city_price(city_name)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `conn.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `connection.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: Now initilizing an agent that has access to tools, specifically the get city price

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 19: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `travel_assistant_agent`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: Invoking the agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 15 | `save_city_price` | function |
| 16 | `get_city_price` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
