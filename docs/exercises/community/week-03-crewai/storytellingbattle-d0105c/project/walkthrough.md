# storytellingbattle: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/storytellingbattle/pyproject.toml](../../../../../../3_crew/community_contributions/storytellingbattle/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/storytellingbattle/README.md](../../../../../../3_crew/community_contributions/storytellingbattle/README.md): Storytellingbattle Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/config/agents.yaml](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/config/agents.yaml): configuration sections `storyteller`, `judge`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/config/tasks.yaml](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/config/tasks.yaml): configuration sections `contestant1`, `contestant2`, `decide`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/__init__.py](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/crew.py](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `Storytellingbattle` | class |
| 15 | `storyteller` | function |
| 22 | `judge` | function |
| 29 | `contestant1` | function |
| 35 | `contestant2` | function |
| 41 | `decide` | function |
| 47 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Storytellingbattle`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/main.py](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `storytellingbattle.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `run` | function |
| 26 | `train` | function |
| 40 | `replay` | function |
| 50 | `test` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `storytellingbattle.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Define `train()`. Its body runs when called.
- Define `replay()`. Its body runs when called.
- Define `test()`. Its body runs when called.
- `Storytellingbattle(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/tools/__init__.py](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/storytellingbattle/src/storytellingbattle/tools/custom_tool.py](../../../../../../3_crew/community_contributions/storytellingbattle/src/storytellingbattle/tools/custom_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `MyCustomToolInput` | class |
| 10 | `MyCustomTool` | class |
| 17 | `_run` | function |

**Read in this order:**

- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MyCustomToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MyCustomTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
