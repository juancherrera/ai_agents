# research outreach app

Source: [2_openai/community_contributions/philip/research_outreach_app.ipynb](../../../../../../../2_openai/community_contributions/philip/research_outreach_app.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Await vs. concurrent work → Streaming sends partial progress → Structured output validates shape → Agent as a tool vs. handoff → Drafting and delivering are separate steps → A callback receives a message and history → Configuration fills named placeholders

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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Follow the source in order

### Cell 1: ✅ Imports & Environment

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `textwrap`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents.model_settings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai.types.responses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email.mime.text`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `smtplib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `GMAIL_USER`. Re-executing this cell updates kernel state.
- Store expression result in `GMAIL_APP_PASSWORD`. Re-executing this cell updates kernel state.
- Store expression result in `GMAIL_TO`. Re-executing this cell updates kernel state.
- Store expression result in `DRY_RUN`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: ✅ Helper Utilities

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Define `run_async(coro)`. Its body runs when called.
- Define `parse_numbered_list(text)`. Its body runs when called.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `questions.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: ✅ Agent Definitions

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store expression result in `clarifier_agent`. Re-executing this cell updates kernel state.
- Define class `SearchQueryPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LeadResearchPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `lead_researcher`. Re-executing this cell updates kernel state.
- Store expression result in `search_executor`. Re-executing this cell updates kernel state.
- Store dictionary in `persona_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agents`. Re-executing this cell updates kernel state.
- Store expression result in `reviewer_agent`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer`. Re-executing this cell updates kernel state.
- Store expression result in `html_converter`. Re-executing this cell updates kernel state.
- Define class `EmailSendReport` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `send_email(subject, html_body)`. Its body runs when called.
- Define `_email_manager_agent()`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.
- `subject_writer.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `html_converter.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: ✅ Core Async Workflow

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Define `generate_clarifying_questions(initial_request)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `build_research_plan(initial_request, clarifications)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `execute_searches(plan)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `draft_email_variations(plan, search_summaries, clarifications)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `select_best_email(drafts)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `format_and_send(winning_body, actually_send)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `run_pipeline(initial_request, clarifications, send_email_flag)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `summaries.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `trace`: Group activity for inspection. Configured tracing may export run data.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: ✅ Gradio Callbacks

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 10: code

- Define `on_generate_questions(initial_request)`. Its body runs when called.
- Define `on_run_pipeline(initial_request, answers, questions, send_email_flag)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: ✅ Launch Gradio Interface

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- `demo.launch`: Start the UI/server; defining the UI alone does not start it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `run_async` | function |
| 4 | `parse_numbered_list` | function |
| 6 | `SearchQueryPlan` | class |
| 6 | `LeadResearchPlan` | class |
| 6 | `EmailSendReport` | class |
| 6 | `send_email` | function |
| 6 | `_email_manager_agent` | function |
| 8 | `generate_clarifying_questions` | async function |
| 8 | `build_research_plan` | async function |
| 8 | `execute_searches` | async function |
| 8 | `draft_email_variations` | async function |
| 8 | `select_best_email` | async function |
| 8 | `format_and_send` | async function |
| 8 | `run_pipeline` | async function |
| 10 | `on_generate_questions` | function |
| 10 | `on_run_pipeline` | function |
| 12 | `_handle_generate` | function |
| 12 | `_handle_run` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
