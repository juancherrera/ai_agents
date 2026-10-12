# financial researcher guard: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/config/agents.yaml](../../../../../../3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/config/agents.yaml): configuration sections `researcher`, `analyst`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/config/tasks.yaml](../../../../../../3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/config/tasks.yaml): configuration sections `research_task`, `analysis_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/crew.py](../../../../../../3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `json` — Encoding/decoding structured text.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `Research` | class |
| 21 | `validate_json_output` | function |
| 31 | `validate_report_content` | function |
| 45 | `FinancialResearcherGuard` | class |
| 58 | `researcher` | function |
| 66 | `analyst` | function |
| 76 | `research_task` | function |
| 86 | `analysis_task` | function |
| 95 | `crew` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Research` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `validate_json_output(result)`. Its body runs when called.
- Define `validate_report_content(result)`. Its body runs when called.
- Define class `FinancialResearcherGuard`. Separate the class definition, constructing an instance, and calling its methods.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/main.py](../../../../../../3_crew/community_contributions/financial_researcher_guard/src/financial_researcher_guard/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `financial_researcher_guard.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `run` | function |
| 32 | `train` | function |
| 46 | `replay` | function |
| 56 | `test` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `financial_researcher_guard.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Define `train()`. Its body runs when called.
- Define `replay()`. Its body runs when called.
- Define `test()`. Its body runs when called.
- `FinancialResearcherGuard(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
