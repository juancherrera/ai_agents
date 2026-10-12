# stock picker advance: project walkthrough

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

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Host and workers have different responsibilities

A distributed host connects workers; workers register agents and handle requests. A stop sequence must clean up both. This logical model opens no port and starts no process.

**Ask yourself:** If both workers register different agents, who handles the judge request?

**Practice:** Add another agent type to worker_a and locate it. Explain how a host connection failure differs from missing registration.

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

- [3_crew/community_contributions/stock-picker-advance/pyproject.toml](../../../../../../3_crew/community_contributions/stock-picker-advance/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/stock-picker-advance/README.md](../../../../../../3_crew/community_contributions/stock-picker-advance/README.md): 🧠📈 CrewAI Stock Picker — Industry-Driven Multi-Agent Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/stock-picker-advance/requirements.txt](../../../../../../3_crew/community_contributions/stock-picker-advance/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.
- [3_crew/community_contributions/stock-picker-advance/src/stock_picker/config/agents.yaml](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/config/agents.yaml): configuration sections `manager`, `industry_mapper`, `company_discovery_agent`, `fundamental_screener`, `valuation_analyst`, `news_risk_analyst`, `shortlist_writer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/stock-picker-advance/src/stock_picker/config/tasks.yaml](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/config/tasks.yaml): configuration sections `map_industry`, `company_discovery`, `screen_fundamentals`, `valuation_rank`, `news_and_risks`, `compile_shortlist`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/stock-picker-advance/scripts/sanity_memory.py](../../../../../../3_crew/community_contributions/stock-picker-advance/scripts/sanity_memory.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `stock_picker.crew` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/stock-picker-advance/src/stock_picker/__init__.py](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/__init__.py)

**Read in this order:**

- Store list in `__all__`. Re-executing this cell updates kernel state.
- Store expression result in `__version__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/stock-picker-advance/src/stock_picker/crew.py](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/crew.py)

**Imports:**

- `.schemas` — Local or third-party dependency; follow its definition/import before execution.
- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory.entity.entity_memory` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory.long_term.long_term_memory` — Roles, tasks, crews, and workflow configuration.
- `crewai.memory.short_term.short_term_memory` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 45 | `StockPickerCrew` | class |
| 54 | `manager` | function |
| 62 | `industry_mapper` | function |
| 72 | `company_discovery_agent` | function |
| 82 | `fundamental_screener` | function |
| 92 | `valuation_analyst` | function |
| 102 | `news_risk_analyst` | function |
| 112 | `shortlist_writer` | function |
| 121 | `map_industry` | function |
| 127 | `company_discovery` | function |
| 134 | `screen_fundamentals` | function |
| 141 | `valuation_rank` | function |
| 148 | `news_and_risks` | function |
| 155 | `compile_shortlist` | function |
| 163 | `crew` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.short_term.short_term_memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.long_term.long_term_memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.entity.entity_memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.schemas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `search_tool`. Re-executing this cell updates kernel state.
- Store expression result in `web_rag`. Re-executing this cell updates kernel state.
- Store expression result in `scraper`. Re-executing this cell updates kernel state.
- Store expression result in `code_tool`. Re-executing this cell updates kernel state.
- Store expression result in `default_llm`. Re-executing this cell updates kernel state.
- Store expression result in `function_llm`. Re-executing this cell updates kernel state.
- Store dictionary in `EMBEDDER_CONFIG`. Re-executing this cell updates kernel state.
- Define class `StockPickerCrew`. Separate the class definition, constructing an instance, and calling its methods.
- `SerperDevTool`: Attach hosted search; it needs configured service access.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/stock-picker-advance/src/stock_picker/main.py](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/main.py)

**Imports:**

- `.crew` — Local or third-party dependency; follow its definition/import before execution.
- `argparse` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `parse_args` | function |

**Read in this order:**

- Make available: `argparse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `parse_args()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `crew.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/stock-picker-advance/src/stock_picker/schemas.py](../../../../../../3_crew/community_contributions/stock-picker-advance/src/stock_picker/schemas.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `CompanyBrief` | class |
| 13 | `ScreenerMetrics` | class |
| 27 | `ValuationView` | class |
| 36 | `RiskNewsItem` | class |
| 44 | `FinalPick` | class |
| 53 | `FinalShortlist` | class |
| 58 | `CompanyBriefList` | class |
| 62 | `ScreenerMetricsList` | class |
| 66 | `ValuationViewList` | class |
| 70 | `RiskNewsItemList` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `CompanyBrief` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ScreenerMetrics` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ValuationView` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RiskNewsItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `FinalPick` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `FinalShortlist` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CompanyBriefList` using . Separate the class definition, constructing an instance, and calling its methods.
- Define class `ScreenerMetricsList` using . Separate the class definition, constructing an instance, and calling its methods.
- Define class `ValuationViewList` using . Separate the class definition, constructing an instance, and calling its methods.
- Define class `RiskNewsItemList` using . Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
