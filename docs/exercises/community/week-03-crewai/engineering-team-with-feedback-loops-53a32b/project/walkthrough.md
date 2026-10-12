# engineering team with feedback loops: project walkthrough

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

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

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

## Configuration and supporting files

- [3_crew/community_contributions/engineering_team_with_feedback_loops/pyproject.toml](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/engineering_team_with_feedback_loops/README.md](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/README.md): Engineering Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/config/agents.yaml](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/config/agents.yaml): configuration sections `development_lead`, `backend_engineer`, `code_reviewer`, `test_engineer`, `frontend_engineer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/config/tasks.yaml](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/config/tasks.yaml): configuration sections `design_task`, `backend_coding_task`, `frontend_coding_task`, `code_review_task`, `frontend_code_review_task`, `test_preparation_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/engineering_team_with_feedback_loops/app.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/app.py)

**Imports:**

- `engineering_team_using_flow.main` — Local or third-party dependency; follow its definition/import before execution.
- `engineering_team_using_flow.shared_queue` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `pydantic` — Validated structured objects.
- `queue` — Local or third-party dependency; follow its definition/import before execution.
- `random` — Demo/simulation randomness.
- `threading` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `generate_random_statement` | function |
| 19 | `start_long_running_process` | function |
| 39 | `run_and_stream` | function |

**Read in this order:**

- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `engineering_team_using_flow.main`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `engineering_team_using_flow.shared_queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `generate_random_statement()`. Its body runs when called.
- Define `start_long_running_process()`. Its body runs when called.
- Define `run_and_stream(module_name, requirements)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `thread.start`: Start the receiver runtime/server/resource.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `time.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.
- `shared_task_output_queue.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/__init__.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/engineering_crew.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/crews/engineering_crew/engineering_crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `litellm` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `CodeReviewFeedback` | class |
| 17 | `EngineeringCrew` | class |
| 27 | `development_lead` | function |
| 31 | `backend_engineer` | function |
| 35 | `code_reviewer` | function |
| 39 | `frontend_engineer` | function |
| 43 | `test_engineer` | function |
| 47 | `design_task` | function |
| 51 | `backend_coding_task` | function |
| 55 | `code_review_task` | function |
| 59 | `frontend_code_review_task` | function |
| 63 | `test_preparation_task` | function |
| 67 | `frontend_coding_task` | function |
| 72 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `litellm`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CodeReviewFeedback` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngineeringCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/main.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/main.py)

**Imports:**

- `.shared_queue` — Local or third-party dependency; follow its definition/import before execution.
- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.flow` — Roles, tasks, crews, and workflow configuration.
- `crewai.flow.flow` — Roles, tasks, crews, and workflow configuration.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `engineering_team_using_flow.crews.engineering_crew.engineering_crew` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `EngineeringState` | class |
| 40 | `EngineeringFlow` | class |
| 338 | `kickoff` | function |
| 343 | `plot` | function |
| 42 | `__init__` | function |
| 48 | `generate_business_requirement` | function |
| 73 | `design_product` | function |
| 109 | `develop_backend` | function |
| 151 | `review_backend_code` | function |
| 205 | `develop_frontend` | function |
| 247 | `review_frontend_code` | function |
| 301 | `write_test_cases` | function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.flow.flow`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.flow`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `engineering_team_using_flow.crews.engineering_crew.engineering_crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.shared_queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngineeringState` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `MAX_REVIEW_ITERATIONS`. Re-executing this cell updates kernel state.
- Store list in `BUSINESS_REQUIREMENTS`. Re-executing this cell updates kernel state.
- Define class `EngineeringFlow` using . Separate the class definition, constructing an instance, and calling its methods.
- Define `kickoff()`. Its body runs when called.
- Define `plot()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `start`: Start the receiver runtime/server/resource.
- `engineering_flow.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Crew`: Combine roles, tasks, and an execution process.
- `mycrew.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `self.state.backend_code_review_feedbacks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.state.frontend_code_review_feedbacks.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/shared_queue.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/shared_queue.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `queue` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `TaskInfo` | class |
| 15 | `add_to_queue` | function |

**Read in this order:**

- Make available: `queue`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TaskInfo` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `shared_task_output_queue`. Re-executing this cell updates kernel state.
- Define `add_to_queue(taskInfo)`. Its body runs when called.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/tools/__init__.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/tools/custom_tool.py](../../../../../../3_crew/community_contributions/engineering_team_with_feedback_loops/src/engineering_team_using_flow/tools/custom_tool.py)

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
