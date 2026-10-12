# Tools Usage Experiments: project walkthrough

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

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Agent as a tool vs. handoff

A tool returns its result to the calling agent. A handoff transfers control to the receiving agent. This trace uses ordinary Python to make that distinction visible.

**Ask yourself:** After the tool finishes, who produces the next step?

**Practice:** Sketch which pattern fits a writer consulting a researcher, and which fits routing a customer to support.

## Configuration and supporting files

- [2_openai/community_contributions/Tools_Usage_Experiments/pyproject.toml](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [2_openai/community_contributions/Tools_Usage_Experiments/README.md](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/README.md): WARNINGS AND DISCLAIMERS. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/customer_support_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/customer_support_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `CustomerSupportAgent` | class |
| 12 | `__init__` | function |
| 24 | `run_task` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CustomerSupportAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/main.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/1_no_tools/main.py)

**Imports:**

- `.customer_support_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `dotenv_path`. Re-executing this cell updates kernel state.
- Make available: `.customer_support_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/customer_support_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/customer_support_agent.py)

**Imports:**

- `.tools.general_information_agent` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.

| Source line | Definition | Kind |
| --- | --- | --- |
| 19 | `CustomerSupportAgent` | class |
| 20 | `__init__` | function |
| 35 | `run_task` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.general_information_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CustomerSupportAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/main.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/main.py)

**Imports:**

- `.customer_support_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `dotenv_path`. Re-executing this cell updates kernel state.
- Make available: `.customer_support_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/tools/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/tools/general_information_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/2_simple_tool/tools/general_information_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `GeneralInformationAgent` | class |
| 15 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `GeneralInformationAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/customer_support_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/customer_support_agent.py)

**Imports:**

- `.tools.courses_of_study_agent` — Local or third-party dependency; follow its definition/import before execution.
- `.tools.general_information_agent` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.

| Source line | Definition | Kind |
| --- | --- | --- |
| 26 | `CustomerSupportAgent` | class |
| 27 | `__init__` | function |
| 43 | `run_task` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.general_information_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.courses_of_study_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CustomerSupportAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/main.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/main.py)

**Imports:**

- `.customer_support_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 41 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `dotenv_path`. Re-executing this cell updates kernel state.
- Make available: `.customer_support_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/courses_of_study_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/courses_of_study_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `CoursesOfStudyAgent` | class |
| 4 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CoursesOfStudyAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/general_information_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/3_two_tools/tools/general_information_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `GeneralInformationAgent` | class |
| 4 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `GeneralInformationAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/customer_support_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/customer_support_agent.py)

**Imports:**

- `.tools.courses_of_study_agent` — Local or third-party dependency; follow its definition/import before execution.
- `.tools.general_information_agent` — Local or third-party dependency; follow its definition/import before execution.
- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `CustomerSupportAgent` | class |
| 7 | `__init__` | function |
| 47 | `run_task` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.general_information_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.courses_of_study_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CustomerSupportAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/main.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/main.py)

**Imports:**

- `.customer_support_agent` — Local or third-party dependency; follow its definition/import before execution.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `main` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `dotenv_path`. Re-executing this cell updates kernel state.
- Make available: `.customer_support_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/courses_of_study_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/courses_of_study_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `CourseOfStudyOutput` | class |
| 26 | `CoursesOfStudyAgent` | class |
| 27 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CourseOfStudyOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CoursesOfStudyAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/general_information_agent.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/4_two_tools_better_output/tools/general_information_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `ContactInformationOutput` | class |
| 20 | `GeneralInformationAgent` | class |
| 21 | `__init__` | function |

**Read in this order:**

- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ContactInformationOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `GeneralInformationAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `self.agent.as_tool`: Make an agent callable as a tool; the caller receives the subagent result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/Tools_Usage_Experiments/__init__.py](../../../../../../2_openai/community_contributions/Tools_Usage_Experiments/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
