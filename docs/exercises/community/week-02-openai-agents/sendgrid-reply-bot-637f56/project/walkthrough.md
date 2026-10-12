# sendgrid reply bot: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

## Configuration and supporting files

- [2_openai/community_contributions/sendgrid-reply-bot/requirements.txt](../../../../../../2_openai/community_contributions/sendgrid-reply-bot/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/sendgrid-reply-bot/phase1_agent_email.py](../../../../../../2_openai/community_contributions/sendgrid-reply-bot/phase1_agent_email.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `uuid` — Conversation identifiers.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `send_email` | function |
| 147 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `uuid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SENDGRID_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `FROM_EMAIL`. Re-executing this cell updates kernel state.
- Store expression result in `TO_EMAIL`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `sg`. Re-executing this cell updates kernel state.
- Define `send_email(email_body, reply_to_address)`. Its body runs when called.
- Store expression result in `professional_agent`. Re-executing this cell updates kernel state.
- Store expression result in `engaging_agent`. Re-executing this cell updates kernel state.
- Store expression result in `busy_agent`. Re-executing this cell updates kernel state.
- Store expression result in `professional_draft`. Re-executing this cell updates kernel state.
- Store expression result in `engaging_draft`. Re-executing this cell updates kernel state.
- Store expression result in `busy_draft`. Re-executing this cell updates kernel state.
- Store expression result in `email_sender`. Re-executing this cell updates kernel state.
- Store expression result in `sales_manager`. Re-executing this cell updates kernel state.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `professional_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `engaging_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `busy_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `mail.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/sendgrid-reply-bot/reply_webhook.py](../../../../../../2_openai/community_contributions/sendgrid-reply-bot/reply_webhook.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `fastapi` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.

| Source line | Definition | Kind |
| --- | --- | --- |
| 28 | `send_reply` | function |
| 63 | `receive_reply` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fastapi`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SENDGRID_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `FROM_EMAIL`. Re-executing this cell updates kernel state.
- Store expression result in `sg`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Define `send_reply(to_email, subject, body)`. Its body runs when called.
- Store expression result in `reply_agent`. Re-executing this cell updates kernel state.
- Define `receive_reply(request)`. Its body runs when called; await/schedule the coroutine to execute async work.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `app.post`: Send an HTTP POST. Notification/email calls have external effects.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `form.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `mail.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
