# Provider adapters, structured output, and guardrails

Source: [2_openai/3_lab3.ipynb](../../../../../2_openai/3_lab3.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Wrap compatible async clients as model adapters → configure agents → define NameCheckOutput → run the checking agent → map its decision to a tripwire.

**Before source execution:** Guardrails depend on the checking model and can miss or falsely flag names. They need their own evaluation. The source protected manager still has real delivery tools.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

## Follow the source in order

### Cell 1: Week 2 Day 3

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `instructions1`. Re-executing this cell updates kernel state.
- Store expression result in `instructions2`. Re-executing this cell updates kernel state.
- Store expression result in `instructions3`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: It's easy to use any models with OpenAI compatible endpoints

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 7: code

- Store expression result in `GEMINI_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `DEEPSEEK_BASE_URL`. Re-executing this cell updates kernel state.
- Store expression result in `GROQ_BASE_URL`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `deepseek_client`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_client`. Re-executing this cell updates kernel state.
- Store expression result in `groq_client`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_model`. Re-executing this cell updates kernel state.
- Store expression result in `gemini_model`. Re-executing this cell updates kernel state.
- Store expression result in `llama3_3_model`. Re-executing this cell updates kernel state.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store expression result in `sales_agent1`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agent2`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agent3`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `description`. Re-executing this cell updates kernel state.
- Store expression result in `tool1`. Re-executing this cell updates kernel state.
- Store expression result in `tool2`. Re-executing this cell updates kernel state.
- Store expression result in `tool3`. Re-executing this cell updates kernel state.
- `sales_agent1.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sales_agent2.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sales_agent3.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Define `send_html_email(subject, html_body)`. Its body runs when called.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Store expression result in `subject_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `html_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer`. Re-executing this cell updates kernel state.
- Store expression result in `subject_tool`. Re-executing this cell updates kernel state.
- Store expression result in `html_converter`. Re-executing this cell updates kernel state.
- Store expression result in `html_tool`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `subject_writer.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `html_converter.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Store list in `email_tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Store expression result in `instructions`. Re-executing this cell updates kernel state.
- Store expression result in `emailer_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Store list in `tools`. Re-executing this cell updates kernel state.
- Store list in `handoffs`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `sales_manager_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `sales_manager`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: Check out the trace:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Define class `NameCheckOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `guardrail_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Define `guardrail_against_name(ctx, agent, message)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `GuardrailFunctionOutput`: Return the guardrail result and tripwire decision.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- Store expression result in `careful_sales_manager`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: Check out the trace:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 22: code

- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: Exercise

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 24: blank separator

No execution needed.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 11 | `send_html_email` | function |
| 18 | `NameCheckOutput` | class |
| 19 | `guardrail_against_name` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
