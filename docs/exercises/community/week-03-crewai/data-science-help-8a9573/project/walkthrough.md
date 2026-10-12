# data science help: project walkthrough

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

## Configuration and supporting files

- [3_crew/community_contributions/data_science_help/pyproject.toml](../../../../../../3_crew/community_contributions/data_science_help/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/data_science_help/README.md](../../../../../../3_crew/community_contributions/data_science_help/README.md): DataScienceHelp Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/data_science_help/src/data_science_help/config/agents.yaml](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/config/agents.yaml): configuration sections `design_agent`, `analyst_agent`, `clean_agent`, `visualization_agent`, `controller_agent`, `frontend_agent`, `tester_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/data_science_help/src/data_science_help/config/tasks.yaml](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/config/tasks.yaml): configuration sections `design_task`, `analyze_task`, `clean_and_apply_task`, `visualize_task`, `frontend_task`, `ui_smoke_test_task`, `control_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/data_science_help/src/data_science_help/__init__.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/data_science_help/src/data_science_help/crew.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `data_science_help.structured_outputs` — Local or third-party dependency; follow its definition/import before execution.
- `data_science_help.tools.tools` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 22 | `DataScienceTeam` | class |
| 30 | `design_agent` | function |
| 38 | `analyst_agent` | function |
| 47 | `clean_agent` | function |
| 58 | `visualization_agent` | function |
| 69 | `frontend_agent` | function |
| 78 | `tester_agent` | function |
| 88 | `controller_agent` | function |
| 97 | `design_task` | function |
| 103 | `analyze_task` | function |
| 110 | `clean_and_apply_task` | function |
| 116 | `visualize_task` | function |
| 123 | `frontend_task` | function |
| 127 | `ui_smoke_test_task` | function |
| 131 | `control_task` | function |
| 138 | `crew` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `data_science_help.tools.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `data_science_help.structured_outputs`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `serper_api_key`. Re-executing this cell updates kernel state.
- Define class `DataScienceTeam`. Separate the class definition, constructing an instance, and calling its methods.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/data_science_help/src/data_science_help/main.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/main.py)

**Imports:**

- `data_science_help.crew` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 37 | `ensure_dirs` | function |
| 45 | `base_inputs` | function |
| 59 | `run` | function |

**Read in this order:**

- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `data_science_help.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `analyze_requirements`. Re-executing this cell updates kernel state.
- Store list in `research_requirements`. Re-executing this cell updates kernel state.
- Define `ensure_dirs()`. Its body runs when called.
- Define `base_inputs()`. Its body runs when called.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `DataScienceTeam(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/data_science_help/src/data_science_help/structured_outputs.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/structured_outputs.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `FunctionSpec` | class |
| 14 | `ClassSpec` | class |
| 18 | `ModuleSpec` | class |
| 24 | `SystemDesign` | class |
| 34 | `DatasetAudit` | class |
| 57 | `ReportCitation` | class |
| 63 | `ReportScope` | class |
| 67 | `DomainReport` | class |
| 97 | `CleaningAction` | class |
| 112 | `CleaningPlan` | class |
| 125 | `CleanedDataRef` | class |
| 144 | `CleaningWrapper` | class |
| 151 | `FigPaths` | class |
| 165 | `VisualSummary` | class |
| 189 | `StepAudit` | class |
| 78 | `min_citations_require` | function |
| 84 | `limit_key_findings` | function |
| 90 | `limit_kpis` | function |
| 119 | `action_require` | function |
| 137 | `check_keys` | function |
| 160 | `empty_lists_to_none` | function |
| 172 | `ensure_one_figure` | function |
| 199 | `_non_empty_reco` | function |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `FunctionSpec` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ClassSpec` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ModuleSpec` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SystemDesign` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `DatasetAudit` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReportCitation` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ReportScope` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `DomainReport` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CleaningAction` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CleaningPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CleanedDataRef` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CleaningWrapper` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `FigPaths` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `VisualSummary` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `StepAudit` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/data_science_help/src/data_science_help/tools/__init__.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/data_science_help/src/data_science_help/tools/custom_tool.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/tools/custom_tool.py)

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

### [3_crew/community_contributions/data_science_help/src/data_science_help/tools/tools.py](../../../../../../3_crew/community_contributions/data_science_help/src/data_science_help/tools/tools.py)

**Imports:**

- `crewai_tools` — Crew search and action integrations.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `get_web_search_tool` | function |
| 15 | `get_code_interpreter_tool` | function |
| 18 | `get_json_search_tool` | function |
| 24 | `get_file_read_tool` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_web_search_tool()`. Its body runs when called.
- Define `get_code_interpreter_tool()`. Its body runs when called.
- Define `get_json_search_tool()`. Its body runs when called.
- Define `get_file_read_tool()`. Its body runs when called.
- `SerperDevTool`: Attach hosted search; it needs configured service access.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
