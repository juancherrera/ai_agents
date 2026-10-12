# course instructor agent: project walkthrough

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

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

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

## Module-by-module reading map

### [2_openai/community_contributions/course_instructor_agent/ccordinator_agent.py](../../../../../../2_openai/community_contributions/course_instructor_agent/ccordinator_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `curriculum_designer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `instruction_designer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `practice_designer_agent` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `qa_agent` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `CoordinatorInstructions` | class |
| 43 | `Course` | class |
| 50 | `CoordinatorAgent` | class |
| 22 | `name` | function |
| 25 | `instructions` | function |
| 36 | `handoff_description` | function |
| 39 | `model` | function |
| 51 | `__init__` | function |
| 58 | `get_agent` | function |
| 61 | `init_agent` | function |
| 77 | `as_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `curriculum_designer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `instruction_designer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `practice_designer_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `qa_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `INSTRUCTIONS`. Re-executing this cell updates kernel state.
- Define class `CoordinatorInstructions`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Course` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CoordinatorAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `self.curriculum_designer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `self.instruction_designer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `self.practice_designer_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.
- `self.qa_agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/course_instructor_agent/course_manager.py](../../../../../../2_openai/community_contributions/course_instructor_agent/course_manager.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `ccordinator_agent` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `openai.types.responses` — Compatible model clients and request/response types.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `run` | async function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ccordinator_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai.types.responses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `Runner.run_streamed`: Begin a streamed agent run; select the relevant event types from its stream.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/course_instructor_agent/curriculum_designer_agent.py](../../../../../../2_openai/community_contributions/course_instructor_agent/curriculum_designer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `CurriculumDesignerInstructions` | class |
| 29 | `Activity` | class |
| 40 | `Lesson` | class |
| 53 | `Module` | class |
| 66 | `Curriculum` | class |
| 76 | `CurriculumDesignerAgent` | class |
| 9 | `name` | function |
| 12 | `instructions` | function |
| 23 | `handoff_description` | function |
| 26 | `model` | function |
| 77 | `__init__` | function |
| 87 | `as_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CurriculumDesignerInstructions`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Activity` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Lesson` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Module` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Curriculum` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CurriculumDesignerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/course_instructor_agent/instruction_designer_agent.py](../../../../../../2_openai/community_contributions/course_instructor_agent/instruction_designer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `InstructionDesignerInstructions` | class |
| 30 | `Instruction` | class |
| 34 | `InstructionDesignerAgent` | class |
| 9 | `name` | function |
| 12 | `instructions` | function |
| 24 | `handoff_description` | function |
| 27 | `model` | function |
| 35 | `__init__` | function |
| 45 | `as_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `InstructionDesignerInstructions`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Instruction` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `InstructionDesignerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/course_instructor_agent/practice_designer_agent.py](../../../../../../2_openai/community_contributions/course_instructor_agent/practice_designer_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `PracticeDesignerInstructions` | class |
| 29 | `Question` | class |
| 34 | `Practice` | class |
| 37 | `PracticeDesignerAgent` | class |
| 9 | `name` | function |
| 12 | `instructions` | function |
| 23 | `handoff_description` | function |
| 26 | `model` | function |
| 38 | `__init__` | function |
| 48 | `as_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `PracticeDesignerInstructions`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Question` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Practice` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PracticeDesignerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/course_instructor_agent/qa_agent.py](../../../../../../2_openai/community_contributions/course_instructor_agent/qa_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `QAAgentInstructions` | class |
| 28 | `QualityAssurance` | class |
| 32 | `QAAgent` | class |
| 9 | `name` | function |
| 12 | `instructions` | function |
| 22 | `handoff_description` | function |
| 25 | `model` | function |
| 33 | `__init__` | function |
| 44 | `as_tool` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `QAAgentInstructions`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `QualityAssurance` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `QAAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
