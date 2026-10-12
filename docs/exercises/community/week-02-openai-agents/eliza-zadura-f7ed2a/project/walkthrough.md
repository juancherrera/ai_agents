# eliza zadura: project walkthrough

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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

## Configuration and supporting files

- [2_openai/community_contributions/eliza_zadura/reply_webhook_plan.md](../../../../../../2_openai/community_contributions/eliza_zadura/reply_webhook_plan.md): Mailjet Reply Webhook Implementation. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [2_openai/community_contributions/eliza_zadura/SETUP.md](../../../../../../2_openai/community_contributions/eliza_zadura/SETUP.md): Mailjet Reply Webhook - Setup Guide. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/eliza_zadura/reply_webhook.py](../../../../../../2_openai/community_contributions/eliza_zadura/reply_webhook.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `dotenv` — Load explicit environment-file configuration.
- `fastapi` — Local or third-party dependency; follow its definition/import before execution.
- `mailjet_rest` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `send_reply` | function |
| 92 | `receive_reply` | async function |
| 137 | `health_check` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `fastapi`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `mailjet_rest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MAILJET_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `MAILJET_SECRET_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `FROM_EMAIL`. Re-executing this cell updates kernel state.
- Store expression result in `FROM_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- Define `send_reply(to_email, to_name, subject, body)`. Its body runs when called.
- Store expression result in `reply_agent`. Re-executing this cell updates kernel state.
- Define `receive_reply(request)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `health_check()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `app.post`: Send an HTTP POST. Notification/email calls have external effects.
- `app.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `mailjet.send.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `form.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
