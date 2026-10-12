# 4 lab4 ollama

Source: [2_openai/community_contributions/Self_Hosted_Web_Search/4_lab4_ollama.ipynb](../../../../../../../2_openai/community_contributions/Self_Hosted_Web_Search/4_lab4_ollama.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Configure a real SDK agent and inspect its tool schema → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Structured output validates shape → Plan → search → synthesize → Drafting and delivering are separate steps

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

## Follow the source in order

### Cell 1: code

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.model_settings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Store expression result in `OLLAMA_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_client`. Re-executing this cell updates kernel state.
- Store expression result in `ollama_model`. Re-executing this cell updates kernel state.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define `custom_web_search()`. Its body runs when called.
- Store expression result in `search_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `display`: Render a value inside a notebook.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: We will now use Structured Outputs, and include a description of the fields

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 7: code

- Store expression result in `HOW_MANY_SEARCHES`. Re-executing this cell updates kernel state.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `WebSearchItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `planner_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Define `send_email(subject, html_body)`. Its body runs when called.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ReportData` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: The next 3 functions will plan and execute the search, using planner_agent and search_agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 14: code

- Define `plan_searches(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `perform_searches(search_plan)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `search(item)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `asyncio.create_task`: Schedule a coroutine as a task so it can run alongside others.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: The next 2 functions write a report and email it

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Define `write_report(query, search_results)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `send_email(report)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: Showtime!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Store expression result in `query`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `custom_web_search` | function |
| 7 | `WebSearchItem` | class |
| 7 | `WebSearchPlan` | class |
| 9 | `send_email` | function |
| 12 | `ReportData` | class |
| 14 | `plan_searches` | async function |
| 14 | `perform_searches` | async function |
| 14 | `search` | async function |
| 16 | `write_report` | async function |
| 16 | `send_email` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
