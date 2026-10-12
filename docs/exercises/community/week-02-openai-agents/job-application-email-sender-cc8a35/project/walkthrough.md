# Job Application Email Sender: project walkthrough

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

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

## Configuration and supporting files

- [2_openai/community_contributions/Job_Application_Email_Sender/requirements.txt](../../../../../../2_openai/community_contributions/Job_Application_Email_Sender/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/Job_Application_Email_Sender/app.py](../../../../../../2_openai/community_contributions/Job_Application_Email_Sender/app.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `base64` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `io` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `streamlit` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 375 | `run_asyncio_tasks` | function |
| 442 | `send_html_email` | function |
| 471 | `send_email_direct` | function |
| 609 | `produce_draft_for_recruiter` | async function |
| 662 | `send_for_recipient_auto` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `io`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base64`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `streamlit`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run_asyncio_tasks(coros)`. Its body runs when called.
- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `sendgrid_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `instructions1`. Re-executing this cell updates kernel state.
- Store expression result in `instructions2`. Re-executing this cell updates kernel state.
- Store expression result in `instructions3`. Re-executing this cell updates kernel state.
- Store expression result in `applicant1`. Re-executing this cell updates kernel state.
- Store expression result in `applicant2`. Re-executing this cell updates kernel state.
- Store expression result in `applicant3`. Re-executing this cell updates kernel state.
- Store expression result in `subject_writer`. Re-executing this cell updates kernel state.
- Store expression result in `html_converter`. Re-executing this cell updates kernel state.
- Define `send_html_email(sender, recipient, subject, html_body, resume_file)`. Its body runs when called.
- Define `send_email_direct(sender, recipient, subject, html_body, resume_file)`. Its body runs when called.
- Store expression result in `emailer_agent`. Re-executing this cell updates kernel state.
- Store list in `tools_for_drafts`. Re-executing this cell updates kernel state.
- Store expression result in `application_manager_drafts`. Re-executing this cell updates kernel state.
- Store expression result in `col1`, `col2`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `template`. Re-executing this cell updates kernel state.
- Define `produce_draft_for_recruiter(name, email)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `generate_col1`, `generate_col2`, `generate_col3`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `st.stop`: Stop the receiver runtime/server/resource.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `applicant1.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `applicant2.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `applicant3.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `log.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `mail.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `subject_writer.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `html_converter.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `resume_file.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `st.session_state.logs.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
