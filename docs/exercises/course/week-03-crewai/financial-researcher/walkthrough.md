# Research crew: evidence then analysis: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

ResearchCrew gives Serper search to the researcher, then an analyst writes output/report.md. Practice uses fictional evidence.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Plan → search → synthesize

The research workflow separates a search plan, collected findings, and a report. Use fixed fictional snippets first so you can inspect how evidence reaches the writer. No current facts are being researched here.

**Ask yourself:** Will the report receive the original query as well as the findings?

**Practice:** Add a third search plan item and a matching fixture. Explain how the original query keeps synthesis focused.

## Configuration and supporting files

- [3_crew/financial_researcher/pyproject.toml](../../../../../3_crew/financial_researcher/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/financial_researcher/README.md](../../../../../3_crew/financial_researcher/README.md): FinancialResearcher Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/financial_researcher/src/financial_researcher/config/agents.yaml](../../../../../3_crew/financial_researcher/src/financial_researcher/config/agents.yaml): configuration sections `researcher`, `analyst`. Match these names with code references and supplied input placeholders.
- [3_crew/financial_researcher/src/financial_researcher/config/tasks.yaml](../../../../../3_crew/financial_researcher/src/financial_researcher/config/tasks.yaml): configuration sections `research_task`, `analysis_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/financial_researcher/src/financial_researcher/__init__.py](../../../../../3_crew/financial_researcher/src/financial_researcher/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/financial_researcher/src/financial_researcher/crew.py](../../../../../3_crew/financial_researcher/src/financial_researcher/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `ResearchCrew` | class |
| 11 | `researcher` | function |
| 19 | `analyst` | function |
| 26 | `research_task` | function |
| 32 | `analysis_task` | function |
| 39 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ResearchCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/financial_researcher/src/financial_researcher/main.py](../../../../../3_crew/financial_researcher/src/financial_researcher/main.py)

**Imports:**

- `financial_researcher.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `run` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `financial_researcher.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `ResearchCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/financial_researcher/src/financial_researcher/tools/__init__.py](../../../../../3_crew/financial_researcher/src/financial_researcher/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/financial_researcher/src/financial_researcher/tools/custom_tool.py](../../../../../3_crew/financial_researcher/src/financial_researcher/tools/custom_tool.py)

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

Use [.environment/.env configuration](../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
