# financial advisory: project walkthrough

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

- [3_crew/community_contributions/financial_advisory/pyproject.toml](../../../../../../3_crew/community_contributions/financial_advisory/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/financial_advisory/README.md](../../../../../../3_crew/community_contributions/financial_advisory/README.md): FinancialAdvisory Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/financial_advisory/src/financial_advisory/config/agents.yaml](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/config/agents.yaml): configuration sections `user_data_summarizer`, `emergency_fund_advisor`, `investment_advisor`, `insurance_advisor`, `reporting_analyst_financial_plan`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/financial_advisory/src/financial_advisory/config/tasks.yaml](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/config/tasks.yaml): configuration sections `summarize_data_task`, `emergency_fund_task`, `investment_task`, `insurance_task`, `financial_plan_report_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/financial_advisory/src/financial_advisory/__init__.py](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/financial_advisory/src/financial_advisory/crew.py](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `FinancialAdvisory` | class |
| 13 | `user_data_summarizer` | function |
| 20 | `emergency_fund_advisor` | function |
| 27 | `investment_advisor` | function |
| 34 | `insurance_advisor` | function |
| 41 | `reporting_analyst_financial_plan` | function |
| 48 | `summarize_data_task` | function |
| 52 | `emergency_fund_task` | function |
| 56 | `investment_task` | function |
| 60 | `insurance_task` | function |
| 64 | `financial_plan_report_task` | function |
| 68 | `financial_advisory_crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `FinancialAdvisory`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/financial_advisory/src/financial_advisory/main.py](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `financial_advisory.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `run` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `financial_advisory.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `FinancialAdvisory(...).financial_advisory_crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/financial_advisory/src/financial_advisory/tools/__init__.py](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/financial_advisory/src/financial_advisory/tools/custom_tool.py](../../../../../../3_crew/community_contributions/financial_advisory/src/financial_advisory/tools/custom_tool.py)

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
