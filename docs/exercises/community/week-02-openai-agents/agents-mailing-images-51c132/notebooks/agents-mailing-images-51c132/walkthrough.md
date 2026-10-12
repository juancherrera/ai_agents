# agents mailing images

Source: [2_openai/community_contributions/agents_mailing_images.ipynb](../../../../../../../2_openai/community_contributions/agents_mailing_images.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Configure a real SDK agent and inspect its tool schema → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → Await vs. concurrent work → Structured output validates shape → Evaluate, give feedback, and bound revision → Agent as a tool vs. handoff → Drafting and delivering are separate steps

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

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

## Follow the source in order

### Cell 1: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.colab`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `userdata.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Store expression result in `instructions1`. Re-executing this cell updates kernel state.
- Store expression result in `instructions2`. Re-executing this cell updates kernel state.
- Store expression result in `instructions3`. Re-executing this cell updates kernel state.
- Store expression result in `description`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agent1`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agent2`. Re-executing this cell updates kernel state.
- Store expression result in `sales_agent3`. Re-executing this cell updates kernel state.
- Store expression result in `sales_manager_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `tool1`. Re-executing this cell updates kernel state.
- Store expression result in `tool2`. Re-executing this cell updates kernel state.
- Store expression result in `tool3`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Store expression result in `sales_manager`. Re-executing this cell updates kernel state.
- Store expression result in `image_generation_tool`. Re-executing this cell updates kernel state.
- Store expression result in `image_agent`. Re-executing this cell updates kernel state.
- Store expression result in `html_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `html_converter`. Re-executing this cell updates kernel state.
- Define class `Ctx` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `send_html_tool(ctx)`. Its body runs when called.
- Store expression result in `mailer_instructions`. Re-executing this cell updates kernel state.
- Store expression result in `emailer_agent`. Re-executing this cell updates kernel state.
- Store expression result in `message`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `sales_agent1.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sales_agent2.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sales_agent3.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 3 | `Ctx` | class |
| 3 | `send_html_tool` | function |
| 3 | `__getitem__` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
