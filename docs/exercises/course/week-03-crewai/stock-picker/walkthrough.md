# Stock picker: structured research and a manager: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

StockPicker uses a hierarchical manager, typed research lists, search tools, notification tool, and persisted memory. Practice makes no investment recommendation or notification.

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

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

## Configuration and supporting files

- [3_crew/stock_picker/pyproject.toml](../../../../../3_crew/stock_picker/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/stock_picker/README.md](../../../../../3_crew/stock_picker/README.md): StockPicker Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/stock_picker/report.md](../../../../../3_crew/stock_picker/report.md): Comprehensive Company Analysis Report. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/stock_picker/src/stock_picker/config/agents.yaml](../../../../../3_crew/stock_picker/src/stock_picker/config/agents.yaml): configuration sections `trending_company_finder`, `financial_researcher`, `stock_picker`, `manager`. Match these names with code references and supplied input placeholders.
- [3_crew/stock_picker/src/stock_picker/config/tasks.yaml](../../../../../3_crew/stock_picker/src/stock_picker/config/tasks.yaml): configuration sections `find_trending_companies`, `research_trending_companies`, `pick_best_company`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/stock_picker/src/stock_picker/__init__.py](../../../../../3_crew/stock_picker/src/stock_picker/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/stock_picker/src/stock_picker/crew.py](../../../../../3_crew/stock_picker/src/stock_picker/crew.py)

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
| 11 | `TrendingCompany` | class |
| 17 | `TrendingCompanyList` | class |
| 21 | `TrendingCompanyResearch` | class |
| 28 | `TrendingCompanyResearchList` | class |
| 34 | `StockPicker` | class |
| 41 | `trending_company_finder` | function |
| 46 | `financial_researcher` | function |
| 51 | `stock_picker` | function |
| 56 | `find_trending_companies` | function |
| 63 | `research_trending_companies` | function |
| 70 | `pick_best_company` | function |
| 79 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools.push_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.rag_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.ltm_sqlite_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TrendingCompany` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompanyList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompanyResearch` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCompanyResearchList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `StockPicker`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/stock_picker/src/stock_picker/main.py](../../../../../3_crew/stock_picker/src/stock_picker/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `stock_picker.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `stock_picker.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `StockPicker(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/stock_picker/src/stock_picker/tools/__init__.py](../../../../../3_crew/stock_picker/src/stock_picker/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/stock_picker/src/stock_picker/tools/push_tool.py](../../../../../3_crew/stock_picker/src/stock_picker/tools/push_tool.py)

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
| 21 | `_run` | function |

**Read in this order:**

- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `PushNotification` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PushNotificationTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
