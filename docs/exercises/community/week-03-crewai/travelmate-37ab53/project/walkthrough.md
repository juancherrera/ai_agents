# travelmate: project walkthrough

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

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/travelmate/pyproject.toml](../../../../../../3_crew/community_contributions/travelmate/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/travelmate/README.md](../../../../../../3_crew/community_contributions/travelmate/README.md): 🧳 TravelMate. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/travelmate/src/travelmate/config/agents.yaml](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/config/agents.yaml): configuration sections `travel_guide_agent`, `food_guide_agent`, `stay_guide_agent`, `travel_planner_agent`, `activity_planner_agent`, `food_planner_agent`, `budget_planner_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/travelmate/src/travelmate/config/tasks.yaml](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/config/tasks.yaml): configuration sections `travel_guide_task`, `food_guide_task`, `stay_guide_task`, `travel_plan_task`, `activity_plan_task`, `food_plan_task`, `budget_plan_task`, `complete_travel_package_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/travelmate/src/travelmate/__init__.py](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/travelmate/src/travelmate/crew.py](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `Travelmate` | class |
| 15 | `travel_guide_agent` | function |
| 22 | `food_guide_agent` | function |
| 29 | `stay_guide_agent` | function |
| 36 | `travel_planner_agent` | function |
| 43 | `activity_planner_agent` | function |
| 50 | `food_planner_agent` | function |
| 57 | `budget_planner_agent` | function |
| 64 | `travel_guide_task` | function |
| 70 | `food_guide_task` | function |
| 76 | `stay_guide_task` | function |
| 82 | `travel_plan_task` | function |
| 88 | `activity_plan_task` | function |
| 94 | `food_plan_task` | function |
| 100 | `budget_plan_task` | function |
| 106 | `complete_travel_package_task` | function |
| 113 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Travelmate`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/travelmate/src/travelmate/main.py](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `travelmate.crew` — Local or third-party dependency; follow its definition/import before execution.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `travelmate.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- `Travelmate(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/travelmate/src/travelmate/tools/__init__.py](../../../../../../3_crew/community_contributions/travelmate/src/travelmate/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
