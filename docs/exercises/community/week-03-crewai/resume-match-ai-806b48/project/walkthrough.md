# resume match ai: project walkthrough

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

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [3_crew/community_contributions/resume_match_ai/pyproject.toml](../../../../../../3_crew/community_contributions/resume_match_ai/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/resume_match_ai/README.md](../../../../../../3_crew/community_contributions/resume_match_ai/README.md): ResumeMatchAi Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/config/agents.yaml](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/config/agents.yaml): configuration sections `resume_analyst`, `job_scraper`, `matchmaker`, `advisor`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/config/tasks.yaml](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/config/tasks.yaml): configuration sections `resume_analysis_task`, `job_scraping_task`, `job_matching_task`, `resume_advising_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/__init__.py](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/__init__.py)

**Imports:**

- `os` — Process configuration and operating-system paths.

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/crew.py](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `resume_match_ai.tools.custom_tool` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `ResumeMatchAi` | class |
| 17 | `resume_analyst` | function |
| 25 | `job_scraper` | function |
| 33 | `matchmaker` | function |
| 41 | `advisor` | function |
| 50 | `resume_analysis_task` | function |
| 57 | `job_scraping_task` | function |
| 64 | `job_matching_task` | function |
| 71 | `resume_advising_task` | function |
| 78 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resume_match_ai.tools.custom_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ResumeMatchAi`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/main.py](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/main.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `resume_match_ai.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `run` | function |
| 28 | `test` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resume_match_ai.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ratelimit_api`. Re-executing this cell updates kernel state.
- Store expression result in `request_token`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- Define `test()`. Its body runs when called.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `ResumeMatchAi(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/tools/__init__.py](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/resume_match_ai/src/resume_match_ai/tools/custom_tool.py](../../../../../../3_crew/community_contributions/resume_match_ai/src/resume_match_ai/tools/custom_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `os` — Process configuration and operating-system paths.
- `pypdf` — Embedded PDF-text extraction.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `download_resume` | function |
| 23 | `extract_resume` | function |

**Read in this order:**

- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`, `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `download_resume(resume_link)`. Its body runs when called.
- Define `extract_resume()`. Its body runs when called.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `file.write`: Write data to a file-like object; inspect the path and mode.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
