# crypto market: project walkthrough

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

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

## Configuration and supporting files

- [3_crew/community_contributions/crypto_market/pyproject.toml](../../../../../../3_crew/community_contributions/crypto_market/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/crypto_market/README.md](../../../../../../3_crew/community_contributions/crypto_market/README.md): CryptoMarket Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/crypto_market/src/crypto_market/config/agents.yaml](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/config/agents.yaml): configuration sections `crypto_news_analyst`, `defi_researcher`, `risk_analyst`, `portfolio_manager`, `manager`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/crypto_market/src/crypto_market/config/tasks.yaml](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/config/tasks.yaml): configuration sections `find_trending_cryptos`, `research_crypto_fundamentals`, `assess_crypto_risks`, `select_crypto_investment`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/crypto_market/src/crypto_market/__init__.py](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/crypto_market/src/crypto_market/crew.py](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/crew.py)

**Imports:**

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
| 11 | `TrendingCrypto` | class |
| 17 | `TrendingCryptoList` | class |
| 21 | `CryptoFundamentals` | class |
| 31 | `CryptoFundamentalsList` | class |
| 35 | `CryptoRiskAssessment` | class |
| 44 | `CryptoRiskAssessmentList` | class |
| 48 | `CryptoInvestmentDecision` | class |
| 58 | `CryptoMarket` | class |
| 65 | `crypto_news_analyst` | function |
| 72 | `defi_researcher` | function |
| 79 | `risk_analyst` | function |
| 86 | `portfolio_manager` | function |
| 93 | `find_trending_cryptos` | function |
| 100 | `research_crypto_fundamentals` | function |
| 107 | `assess_crypto_risks` | function |
| 114 | `select_crypto_investment` | function |
| 121 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.rag_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.memory.storage.ltm_sqlite_storage`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TrendingCrypto` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TrendingCryptoList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoFundamentals` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoFundamentalsList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoRiskAssessment` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoRiskAssessmentList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoInvestmentDecision` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CryptoMarket`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/crypto_market/src/crypto_market/main.py](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/main.py)

**Imports:**

- `crypto_market.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crypto_market.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `CryptoMarket(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/crypto_market/src/crypto_market/tools/__init__.py](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/crypto_market/src/crypto_market/tools/custom_tool.py](../../../../../../3_crew/community_contributions/crypto_market/src/crypto_market/tools/custom_tool.py)

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
