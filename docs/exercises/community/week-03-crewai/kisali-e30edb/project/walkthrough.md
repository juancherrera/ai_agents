# kisali: project walkthrough

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

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

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [3_crew/community_contributions/kisali/coder_agent/pyproject.toml](../../../../../../3_crew/community_contributions/kisali/coder_agent/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/kisali/coder_agent/README.md](../../../../../../3_crew/community_contributions/kisali/coder_agent/README.md): CoderAgent Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/config/agents.yaml](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/config/agents.yaml): configuration sections `python_coder_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/config/tasks.yaml](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/config/tasks.yaml): configuration sections `python_coding_task`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/debate/pyproject.toml](../../../../../../3_crew/community_contributions/kisali/debate/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/kisali/debate/README.md](../../../../../../3_crew/community_contributions/kisali/debate/README.md): Debate Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/kisali/debate/src/debate/config/agents.yaml](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/config/agents.yaml): configuration sections `opposing_debater_agent`, `proposing_debater_agent`, `judge_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/debate/src/debate/config/tasks.yaml](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/config/tasks.yaml): configuration sections `propose`, `oppose`, `decide`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/financial_researcher/pyproject.toml](../../../../../../3_crew/community_contributions/kisali/financial_researcher/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/kisali/financial_researcher/README.md](../../../../../../3_crew/community_contributions/kisali/financial_researcher/README.md): FinancialResearcher Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/config/agents.yaml](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/config/agents.yaml): configuration sections `financial_researcher`, `financial_analyst`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/config/tasks.yaml](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/config/tasks.yaml): configuration sections `research_task`, `analysis_task`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/stock_picker/pyproject.toml](../../../../../../3_crew/community_contributions/kisali/stock_picker/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/kisali/stock_picker/README.md](../../../../../../3_crew/community_contributions/kisali/stock_picker/README.md): StockPicker Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/config/agents.yaml](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/config/agents.yaml): configuration sections `trending_company_finder`, `financial_researcher`, `stock_picker`, `manager`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/config/tasks.yaml](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/config/tasks.yaml): configuration sections `find_trending_companies`, `research_trending_companies`, `pick_best_company`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/swe_team/pyproject.toml](../../../../../../3_crew/community_contributions/kisali/swe_team/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/kisali/swe_team/README.md](../../../../../../3_crew/community_contributions/kisali/swe_team/README.md): SweTeam Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/kisali/swe_team/src/swe_team/config/agents.yaml](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/config/agents.yaml): configuration sections `engineering_lead`, `frontend_engineer`, `backend_engineer`, `test_engineer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/kisali/swe_team/src/swe_team/config/tasks.yaml](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/config/tasks.yaml): configuration sections `design_task`, `backend_coding_task`, `frontend_coding_task`, `test_coding_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/__init__.py](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/crew.py](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `CoderAgent` | class |
| 12 | `python_coder_agent` | function |
| 23 | `python_coding_task` | function |
| 29 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CoderAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/main.py](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/main.py)

**Imports:**

- `coder_agent.crew` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `run` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `coder_agent.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `assignment`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- `CoderAgent(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/tools/__init__.py](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/coder_agent/src/coder_agent/tools/custom_tool.py](../../../../../../3_crew/community_contributions/kisali/coder_agent/src/coder_agent/tools/custom_tool.py)

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

### [3_crew/community_contributions/kisali/debate/src/debate/__init__.py](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/debate/src/debate/crew.py](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `Debate` | class |
| 12 | `opposing_debater_agent` | function |
| 19 | `proposing_debater_agent` | function |
| 26 | `judge_agent` | function |
| 33 | `propose` | function |
| 39 | `oppose` | function |
| 45 | `decide` | function |
| 51 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Debate`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/debate/src/debate/main.py](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `debate.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `debate.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- `Debate(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/debate/src/debate/tools/__init__.py](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/debate/src/debate/tools/custom_tool.py](../../../../../../3_crew/community_contributions/kisali/debate/src/debate/tools/custom_tool.py)

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

### [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/__init__.py](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/crew.py](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `FinancialResearcher` | class |
| 10 | `financial_researcher` | function |
| 18 | `financial_analyst` | function |
| 25 | `research_task` | function |
| 31 | `analysis_task` | function |
| 38 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `FinancialResearcher`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/main.py](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/main.py)

**Imports:**

- `financial_researcher.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `run` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `financial_researcher.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `FinancialResearcher(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/tools/__init__.py](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/tools/custom_tool.py](../../../../../../3_crew/community_contributions/kisali/financial_researcher/src/financial_researcher/tools/custom_tool.py)

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

### [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/__init__.py](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/crew.py](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/crew.py)

**Imports:**

- `.tools.push_tool` — Local or third-party dependency; follow its definition/import before execution.
- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory.storage.ltm_sqlite_storage` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory.storage.rag_storage` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `TrendingCompanies` | class |
| 17 | `TrendingCompaniesList` | class |
| 21 | `TrendingCompaniesResearch` | class |
| 28 | `TrendingCompaniesResearchList` | class |
| 33 | `StockPicker` | class |
| 40 | `trending_company_finder` | function |
| 48 | `financial_researcher` | function |
| 55 | `stock_picker` | function |
| 63 | `find_trending_companies` | function |
| 70 | `research_trending_companies` | function |
| 77 | `pick_best_company` | function |
| 83 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.push_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.rag_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.ltm_sqlite_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TrendingCompanies` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompaniesList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompaniesResearch` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompaniesResearchList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `StockPicker`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/main.py](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/main.py)

**Imports:**

- `stock_picker.crew` — Local or third-party dependency; follow its definition/import before execution.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `run` | function |

**Read in this order:**

- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `stock_picker.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `StockPicker(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/tools/__init__.py](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/stock_picker/src/stock_picker/tools/push_tool.py](../../../../../../3_crew/community_contributions/kisali/stock_picker/src/stock_picker/tools/push_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `requests` — Synchronous HTTP requests.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `PushNotification` | class |
| 12 | `PushNotificationTool` | class |
| 18 | `_run` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `PushNotification` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PushNotificationTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/swe_team/src/swe_team/__init__.py](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/swe_team/src/swe_team/crew.py](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `SweTeam` | class |
| 13 | `engineering_lead` | function |
| 19 | `frontend_engineer` | function |
| 26 | `test_engineer` | function |
| 37 | `backend_engineer` | function |
| 48 | `design_task` | function |
| 54 | `backend_coding_task` | function |
| 60 | `frontend_coding_task` | function |
| 66 | `test_coding_task` | function |
| 72 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SweTeam`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/swe_team/src/swe_team/main.py](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/main.py)

**Imports:**

- `swe_team.crew` — Local or third-party dependency; follow its definition/import before execution.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 23 | `run` | function |

**Read in this order:**

- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `swe_team.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `requirements`. Re-executing this cell updates kernel state.
- Store expression result in `class_name`. Re-executing this cell updates kernel state.
- Store expression result in `module_name`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `SweTeam(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/swe_team/src/swe_team/tools/__init__.py](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/kisali/swe_team/src/swe_team/tools/custom_tool.py](../../../../../../3_crew/community_contributions/kisali/swe_team/src/swe_team/tools/custom_tool.py)

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
