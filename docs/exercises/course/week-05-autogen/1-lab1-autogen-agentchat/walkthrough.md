# Assistant, message, model client, and ticket tool

Source: [5_autogen/1_lab1_autogen_agentchat.ipynb](../../../../../5_autogen/1_lab1_autogen_agentchat.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Construct model client → construct TextMessage → invoke AssistantAgent → create ticket lookup → attach it as a tool → reflect on its result.

**Before source execution:** The database setup deletes tickets.db if it already exists. The practice database is in memory and never imports or resets the original database.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

## Follow the source in order

### Cell 1: Welcome to Week 5 Day 1

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: First concept: the Model

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Make available: `autogen_ext.models.openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `model_client`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Make available: `autogen_ext.models.ollama`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ollamamodel_client`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: Second concept: The Message

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 7: code

- Make available: `autogen_agentchat.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- `TextMessage`: Construct a typed text message with its source.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Third concept: The Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `agent`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: Put it all together with on_messages

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Make available: `autogen_core`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `response`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: Let's make a local database of ticket prices

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `conn`. Re-executing this cell updates kernel state.
- Store expression result in `c`. Re-executing this cell updates kernel state.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `c.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `conn.close`: Release the resource; match setup with cleanup.
- `os.remove`: Delete an item/file depending on the receiver. Check the target.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Define `save_city_price(city_name, round_trip_price)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `c.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.commit`: Persist pending database changes or another transaction.
- `conn.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Define `get_city_price(city_name)`. Its body runs when called.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `c.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `conn.close`: Release the resource; match setup with cleanup.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Make available: `autogen_agentchat.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `smart_agent`. Re-executing this cell updates kernel state.
- `AssistantAgent`: Configure an AutoGen assistant and its model client/tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 14 | `save_city_price` | function |
| 15 | `get_city_price` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
