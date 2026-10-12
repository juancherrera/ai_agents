# 04 weathermate

Source: [1_foundations/community_contributions/4_weathermate_agent/04_weathermate.ipynb](../../../../../../../1_foundations/community_contributions/4_weathermate_agent/04_weathermate.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Streaming sends partial progress → Evaluate, give feedback, and bound revision → A callback receives a message and history → Request, status, and parsed body

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: ☀️🏃‍♀️ WeatherMate

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Explanation and external references (remote contents unverified)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `weather_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `ticketmaster_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `today_str`. Re-executing this cell updates kernel state.
- Store expression result in `day_name`. Re-executing this cell updates kernel state.
- Store expression result in `nb_activity`. Re-executing this cell updates kernel state.
- Store formatted string in `system_message`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Define class `WeatherAPI`. Separate the class definition, constructing an instance, and calling its methods.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `forecast.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Make available: `abc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `BaseEventAPI` using `ABC`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TicketmasterAPI` using `BaseEventAPI`. Separate the class definition, constructing an instance, and calling its methods.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `event.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define class `ChatAssistant`. Separate the class definition, constructing an instance, and calling its methods.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `stored_values.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `responses.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Define class `GradioInterface`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ActivityAssistant`. Separate the class definition, constructing an instance, and calling its methods.
- `gr.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `gradio_interface.launch`: Start the UI/server; defining the UI alone does not start it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

Empty code cell; it defines no state.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 5 | `WeatherAPI` | class |
| 5 | `get_weather` | function |
| 6 | `BaseEventAPI` | class |
| 6 | `TicketmasterAPI` | class |
| 6 | `get_events` | function |
| 6 | `get_events` | function |
| 7 | `ChatAssistant` | class |
| 7 | `__init__` | function |
| 7 | `chat` | function |
| 7 | `handle_tool_call` | function |
| 8 | `GradioInterface` | class |
| 8 | `ActivityAssistant` | class |
| 8 | `__init__` | function |
| 8 | `launch` | function |
| 8 | `__init__` | function |
| 8 | `chat` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
