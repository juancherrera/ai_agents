# trip daily planner: project walkthrough

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

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

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

## Configuration and supporting files

- [3_crew/community_contributions/trip_daily_planner/pyproject.toml](../../../../../../3_crew/community_contributions/trip_daily_planner/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/trip_daily_planner/README.md](../../../../../../3_crew/community_contributions/trip_daily_planner/README.md): Trip Daily Planner with Traveler's Information Input and Email Sending Tool. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/trip_daily_planner/requirements.txt](../../../../../../3_crew/community_contributions/trip_daily_planner/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.
- [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/config/agents.yaml](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/config/agents.yaml): configuration sections `personalized_activity_planner`, `restaurant_scout`, `itinerary_compiler`, `email_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/config/tasks.yaml](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/config/tasks.yaml): configuration sections `personalized_activity_planning_task`, `restaurant_location_scout_task`, `itinerary_compilation_task`, `format_send_email_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/__init__.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/app.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/app.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `main` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `run` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `main`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run(destination_textbox, flight_textbox, hotel_textbox, trip_duration_textbox, personal_interests_textbox, dining_preferences_restrictions_textbox, email_textbox)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/crew.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `pydantic` — Validated structured objects.
- `tools.email_tool` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `Activity` | class |
| 17 | `Restaurant` | class |
| 23 | `DayPlan` | class |
| 28 | `Itinerary` | class |
| 34 | `TripDailyPlanner` | class |
| 41 | `personalized_activity_planner` | function |
| 54 | `restaurant_scout` | function |
| 66 | `itinerary_compiler` | function |
| 74 | `email_agent` | function |
| 82 | `personalized_activity_planning_task` | function |
| 88 | `restaurant_location_scout_task` | function |
| 94 | `itinerary_compilation_task` | function |
| 101 | `format_send_email_task` | function |
| 107 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools.email_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Activity` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Restaurant` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `DayPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Itinerary` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TripDailyPlanner`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/main.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/main.py)

**Imports:**

- `crew` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `run` | function |
| 37 | `run_with_inputs` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Define `run_with_inputs(inputs)`. Its body runs when called.
- `TripDailyPlanner(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/tools/__init__.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/tools/email_tool.py](../../../../../../3_crew/community_contributions/trip_daily_planner/src/trip_daily_planner/tools/email_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `resend` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `EmailModel` | class |
| 15 | `SendEmailTool` | class |
| 22 | `_run` | function |

**Read in this order:**

- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resend`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EmailModel` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SendEmailTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `response.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
