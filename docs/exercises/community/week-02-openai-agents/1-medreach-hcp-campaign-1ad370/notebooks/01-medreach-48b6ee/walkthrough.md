# 01 medreach

Source: [2_openai/community_contributions/1_medreach_hcp_campaign/01_medreach.ipynb](../../../../../../../2_openai/community_contributions/1_medreach_hcp_campaign/01_medreach.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Configure a real SDK agent and inspect its tool schema → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Structured output validates shape → Evaluate, give feedback, and bound revision → A guardrail makes a stop/continue decision → Agent as a tool vs. handoff → Plan → search → synthesize → Drafting and delivering are separate steps → Request, status, and parsed body

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: 📢 MedReach

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: 1. 🧰Imports and Environment Setup

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `RESEND_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `TO_EMAIL`. Re-executing this cell updates kernel state.
- Store expression result in `NAME`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `file_path_input`. Re-executing this cell updates kernel state.
- Store expression result in `FROM_EMAIL`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `GROQ_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `OPENROUTER_MODEL`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `GROQ_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `OPENROUTER_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `groq_client`. Re-executing this cell updates kernel state.
- Store expression result in `openrouter_client`. Re-executing this cell updates kernel state.
- Store expression result in `llama3_3_model`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `openrouter_client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: 2. 🏭Create Fake Data

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store dictionary in `doctors_data`. Re-executing this cell updates kernel state.
- Store expression result in `df`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: 3. 🛠️Function Tools: Data Operations

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Define class `Doctor` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Define `read_csv(file_path_input)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define `filter_audience(doctors_list, specialty, region)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: 4. ✍️Create Writer Agents (Agent-as-Tool Pattern)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Store formatted string in `WRITER_FORMAT_INSTRUCTIONS`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Store formatted string in `evidence_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `evidence_writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Store formatted string in `roi_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `roi_writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Store formatted string in `safety_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `safety_writer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- Store expression result in `evidence_tool`. Re-executing this cell updates kernel state.
- Store expression result in `roi_tool`. Re-executing this cell updates kernel state.
- Store expression result in `safety_tool`. Re-executing this cell updates kernel state.
- Store list in `agent_writer_tools`. Re-executing this cell updates kernel state.
- `evidence_writer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `roi_writer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `safety_writer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 22: code

- Store expression result in `all_tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: 5. 📨Email Formatting Agents

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 24: code

- Store expression result in `subject_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `html_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer`. Re-executing this cell updates kernel state.
- Store expression result in `html_converter`. Re-executing this cell updates kernel state.
- Store expression result in `subject_tool`. Re-executing this cell updates kernel state.
- Store expression result in `html_tool`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `subject_writer.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `html_converter.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: code

- Define `send_email(recipient_email, recipient_name, subject, html_body)`. Its body runs when called.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 26: code

- Define `log_to_csv(campaign_id, doctor_id, doctor_name, email_variant, status)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 27: 6. 📬Create Email Sender Agent

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 28: code

- Store expression result in `email_sender_instructions`. Re-executing this cell updates kernel state.
- Store list in `email_sender_tools`. Re-executing this cell updates kernel state.
- Store expression result in `email_sender_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 29: code

- Store dictionary in `agent_dict`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 30: 7. 🛡️ Create Input Guardrail - Audience Validation

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 31: code

- Define class `CampaignRequestValidation` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `request_validation_agent`. Re-executing this cell updates kernel state.
- Define `validate_campaign_request(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 32: 8. 🧠Create the Campaign Manager Agent (Orchestrator)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 33: code

- Store formatted string in `campaign_manager_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `campaign_manager`. Re-executing this cell updates kernel state.
- Store dictionary in `agent_dict`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 34: 9. 🚀Run the Multi-Agent Campaign System

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 35: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 36: 💡 Bonus: Function vs Agent Tools

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 37: Function tool - FREE & FAST

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 11 | `Doctor` | class |
| 12 | `read_csv` | function |
| 13 | `filter_audience` | function |
| 25 | `send_email` | function |
| 26 | `log_to_csv` | function |
| 31 | `CampaignRequestValidation` | class |
| 31 | `validate_campaign_request` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
