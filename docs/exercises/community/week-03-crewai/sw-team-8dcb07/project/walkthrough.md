# sw team: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

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

## Configuration and supporting files

- [3_crew/community_contributions/sw_team/eng_team_flow/pyproject.toml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/sw_team/eng_team_flow/README.md](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/README.md): Software Engineer Team. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/config/agents.yaml): configuration sections `business_personal`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/config/tasks.yaml): configuration sections `business_usecase_generation_task`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/config/agents.yaml): configuration sections `software_engineer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/config/tasks.yaml): configuration sections `generate_code`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/config/agents.yaml): configuration sections `tech_lead`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/config/tasks.yaml): configuration sections `review_code`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/config/agents.yaml): configuration sections `technical_manager`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/config/tasks.yaml): configuration sections `generate_engineering_task_list`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/pyproject.toml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/README.md](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/README.md): Software Engineer Team. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/config/agents.yaml): configuration sections `business_personal`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/config/tasks.yaml): configuration sections `business_usecase_generation_task`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/config/agents.yaml): configuration sections `software_engineer`, `tech_lead`, `code_summarizer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/config/tasks.yaml): configuration sections `generate_code`, `review_code`, `summarize_code`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/config/agents.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/config/agents.yaml): configuration sections `technical_manager`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/config/tasks.yaml](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/config/tasks.yaml): configuration sections `generate_engineering_task_list`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/sw_team/README.md](../../../../../../3_crew/community_contributions/sw_team/README.md): Software Engineer Team. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/business_crew.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/business/business_crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `BusinessUseCase` | class |
| 13 | `BusinessCrew` | class |
| 22 | `business_personal` | function |
| 29 | `business_usecase_generation_task` | function |
| 35 | `track_collaboration_from_business` | function |
| 40 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `BusinessUseCase` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `BusinessCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/software_engineer.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/software/software_engineer.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 23 | `TechLeadReview` | class |
| 30 | `SoftwareEngineer` | class |
| 84 | `TechLead` | class |
| 38 | `__init__` | function |
| 50 | `software_engineer` | function |
| 65 | `generate_code` | function |
| 74 | `crew` | function |
| 93 | `tech_lead` | function |
| 102 | `review_code` | function |
| 111 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TechLeadReview` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SoftwareEngineer`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TechLead`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/tech_lead.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/engineering/tech_lead/tech_lead.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 23 | `TechLeadReview` | class |
| 30 | `TechLead` | class |
| 39 | `tech_lead` | function |
| 54 | `review_code` | function |
| 63 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TechLeadReview` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TechLead`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/technical_manager.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/crews/manager/technical/technical_manager.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 22 | `EngineeringDesignTask` | class |
| 27 | `EngineeringDesignTaskList` | class |
| 35 | `TechnicalManagerCrew` | class |
| 45 | `technical_manager` | function |
| 54 | `generate_engineering_task_list` | function |
| 62 | `track_collaboration_from_manager` | function |
| 67 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngineeringDesignTask` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngineeringDesignTaskList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TechnicalManagerCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/main.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/main.py)

**Imports:**

- `crewai.flow` — Roles, tasks, crews, and workflow configuration.
- `eng_team_flow.crews.business.business_crew` — Local or third-party dependency; follow its definition/import before execution.
- `eng_team_flow.crews.engineering.software.software_engineer` — Local or third-party dependency; follow its definition/import before execution.
- `eng_team_flow.crews.engineering.tech_lead.tech_lead` — Local or third-party dependency; follow its definition/import before execution.
- `eng_team_flow.crews.manager.technical.technical_manager` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `EngTeamState` | class |
| 33 | `EngTeamFlow` | class |
| 125 | `kickoff` | function |
| 39 | `generate_business_usecase` | function |
| 59 | `generate_engineering_task_list` | function |
| 87 | `generate_code` | function |
| 105 | `review_code` | function |

**Read in this order:**

- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.flow`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.business.business_crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.manager.technical.technical_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.engineering.software.software_engineer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.engineering.tech_lead.tech_lead`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngTeamState` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngTeamFlow` using . Separate the class definition, constructing an instance, and calling its methods.
- Define `kickoff()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `start`: Start the receiver runtime/server/resource.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `eng_team_flow.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `BusinessCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `TechnicalManagerCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `SoftwareEngineer(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `TechLead(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/tools/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/tools/custom_tool.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow/src/eng_team_flow/tools/custom_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `MyCustomToolInput` | class |
| 13 | `MyCustomTool` | class |
| 20 | `_run` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MyCustomToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MyCustomTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/business_crew.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/business/business_crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `BusinessUseCase` | class |
| 13 | `BusinessCrew` | class |
| 22 | `business_personal` | function |
| 29 | `business_usecase_generation_task` | function |
| 35 | `track_collaboration_from_business` | function |
| 40 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `BusinessUseCase` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `BusinessCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/software_engineer.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/engineering/software/software_engineer.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 23 | `Summary` | class |
| 32 | `SoftwareEngineer` | class |
| 40 | `__init__` | function |
| 52 | `software_engineer` | function |
| 65 | `tech_lead` | function |
| 78 | `code_summarizer` | function |
| 91 | `generate_code` | function |
| 100 | `review_code` | function |
| 108 | `summarize_code` | function |
| 117 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Summary` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SoftwareEngineer`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/technical_manager.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/crews/manager/technical/technical_manager.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 22 | `EngineeringDesignTask` | class |
| 27 | `EngineeringDesignTaskList` | class |
| 35 | `TechnicalManagerCrew` | class |
| 45 | `technical_manager` | function |
| 54 | `generate_engineering_task_list` | function |
| 62 | `track_collaboration_from_manager` | function |
| 67 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngineeringDesignTask` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngineeringDesignTaskList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TechnicalManagerCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/main.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/main.py)

**Imports:**

- `crewai.flow` — Roles, tasks, crews, and workflow configuration.
- `eng_team_flow.crews.business.business_crew` — Local or third-party dependency; follow its definition/import before execution.
- `eng_team_flow.crews.engineering.software.software_engineer` — Local or third-party dependency; follow its definition/import before execution.
- `eng_team_flow.crews.manager.technical.technical_manager` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `EngTeamState` | class |
| 30 | `EngTeamFlow` | class |
| 114 | `kickoff` | function |
| 36 | `generate_business_usecase` | function |
| 56 | `generate_engineering_task_list` | function |
| 79 | `generate_code` | function |

**Read in this order:**

- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.flow`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.business.business_crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.manager.technical.technical_manager`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `eng_team_flow.crews.engineering.software.software_engineer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngTeamState` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngTeamFlow` using . Separate the class definition, constructing an instance, and calling its methods.
- Define `kickoff()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `start`: Start the receiver runtime/server/resource.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `eng_team_flow.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `BusinessCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `TechnicalManagerCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `SoftwareEngineer(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/tools/__init__.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/tools/custom_tool.py](../../../../../../3_crew/community_contributions/sw_team/eng_team_flow_se_agents_combined/src/eng_team_flow/tools/custom_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `MyCustomToolInput` | class |
| 13 | `MyCustomTool` | class |
| 20 | `_run` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MyCustomToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MyCustomTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
