# assesment

Source: [2_openai/community_contributions/solisoma/assesment.ipynb](../../../../../../../2_openai/community_contributions/solisoma/assesment.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Messages → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Streaming sends partial progress → Structured output validates shape → Agent as a tool vs. handoff → Plan → search → synthesize → Drafting and delivering are separate steps → A callback receives a message and history

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

## Follow the source in order

### Cell 1: code

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.model_settings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store dictionary in `STEPS`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `CLARIFICATION_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `clarification_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store formatted string in `QUESTION_GENERATION_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `question_generation_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `SEARCH_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Store expression result in `WRITER_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `EMAIL_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define `send_email(subject, html_body, to_email)`. Its body runs when called.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `PLANNER_INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `clarification_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `question_generation_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `search_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `writer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Define `deep_research_chat(message, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run_streamed`: Begin a streamed agent run; select the relevant event types from its stream.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 3 | `WebSearchItem` | class |
| 3 | `WebSearchPlan` | class |
| 3 | `ReportData` | class |
| 8 | `send_email` | function |
| 10 | `deep_research_chat` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
