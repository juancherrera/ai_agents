# AI Clinic hopeogbons: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [2_openai/community_contributions/AI_Clinic_hopeogbons/README.md](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/README.md): 🏥 AI Clinic - Multi-Agent Medical Consultation System. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [2_openai/community_contributions/AI_Clinic_hopeogbons/requirements.txt](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [2_openai/community_contributions/AI_Clinic_hopeogbons/app.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/app.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `chief_physician_agent` — Local or third-party dependency; follow its definition/import before execution.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `resident_physician_agent` — Local or third-party dependency; follow its definition/import before execution.
- `triage_nurse_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `AIClinicManager` | class |
| 266 | `chat_async` | async function |
| 304 | `chat` | function |
| 17 | `__init__` | function |
| 23 | `reset` | function |
| 35 | `process_message` | async function |
| 56 | `_handle_triage` | async function |
| 80 | `_handle_resident` | async function |
| 115 | `_handle_chief` | async function |
| 196 | `_handle_results_delivery` | async function |
| 221 | `_run_specialist_consultation` | async function |
| 360 | `get_status_markdown` | function |
| 403 | `submit_message` | function |
| 419 | `reset_and_update` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `triage_nurse_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `resident_physician_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `chief_physician_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `AIClinicManager`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `manager`. Re-executing this cell updates kernel state.
- Define `chat_async(message, history)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `chat(message, history)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `self.reset`: Reset state. For accounts, this can replace stored data.
- `self.triage_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.resident_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `manager.reset`: Reset state. For accounts, this can replace stored data.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.triage_data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.chief_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.resident_data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/chief_physician_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/chief_physician_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `consultants.emergeny_specialist_agent` — Local or third-party dependency; follow its definition/import before execution.
- `consultants.medicine_specialist_agent` — Local or third-party dependency; follow its definition/import before execution.
- `consultants.surgery_specialist_agent` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 62 | `ChiefPhysicianAgent` | class |
| 63 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `consultants.emergeny_specialist_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `consultants.medicine_specialist_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `consultants.surgery_specialist_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ChiefPhysicianAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.
- `emergency_specialist.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `medicine_specialist.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `surgery_specialist.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/config.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/config.py)

**Read in this order:**

- Store expression result in `TRIAGE_NURSE_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `RESIDENT_PHYSICIAN_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `CHIEF_PHYSICIAN_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `EMERGENCY_SPECIALIST_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `MEDICINE_SPECIALIST_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `SURGERY_SPECIALIST_NAME`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/emergeny_specialist_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/emergeny_specialist_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 26 | `EmergencySpecialistHandoff` | class |
| 35 | `EmergencySpecialistAgent` | class |
| 36 | `__init__` | function |
| 45 | `__call__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `EmergencySpecialistHandoff` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EmergencySpecialistAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/medicine_specialist_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/medicine_specialist_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 28 | `MedicineSpecialistHandoff` | class |
| 38 | `MedicineSpecialistAgent` | class |
| 39 | `__init__` | function |
| 48 | `__call__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `MedicineSpecialistHandoff` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MedicineSpecialistAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/surgery_specialist_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/consultants/surgery_specialist_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 28 | `SurgerySpecialistHandoff` | class |
| 39 | `SurgerySpecialistAgent` | class |
| 40 | `__init__` | function |
| 49 | `__call__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `SurgerySpecialistHandoff` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SurgerySpecialistAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/resident_physician_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/resident_physician_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 56 | `ResidentPhysicianAgent` | class |
| 57 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `ResidentPhysicianAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/AI_Clinic_hopeogbons/triage_nurse_agent.py](../../../../../../2_openai/community_contributions/AI_Clinic_hopeogbons/triage_nurse_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `config` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 40 | `TriageNurseAgent` | class |
| 41 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store formatted string in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `TriageNurseAgent` using `Agent`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
