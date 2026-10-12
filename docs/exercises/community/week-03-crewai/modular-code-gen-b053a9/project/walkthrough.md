# modular code gen: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Cache and simulation are not live prices

The trading exercises use market data and cached values. A fixed fictional quote lets you observe caching. A fallback simulation must stay identifiable as simulated data.

**Ask yourself:** How many provider-fixture calls occur for the same cached symbol?

**Practice:** Call a second symbol. Explain how you would record a quote timestamp and expire old cache entries.

### Simulated accounts change state

Buying shares updates cash, holdings, and a transaction record. Practice against a dictionary so you do not import account modules that create or alter databases.

**Ask yourself:** How much cash remains after buying three fictional shares at 100 each?

**Practice:** Try an unaffordable order and confirm the balance is unchanged. Explain why reset is a data-changing action.

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

- [3_crew/community_contributions/modular_code_gen/pyproject.toml](../../../../../../3_crew/community_contributions/modular_code_gen/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/modular_code_gen/README.md](../../../../../../3_crew/community_contributions/modular_code_gen/README.md): ModularCodeGen Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/config/agents.yaml](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/config/agents.yaml): configuration sections `engineering_lead`, `backend_engineer`, `frontend_engineer`, `file_writer`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/config/tasks.yaml](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/config/tasks.yaml): configuration sections `design_task`, `code_task`, `writer_task_back`, `frontend_task`, `writer_task_front`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/modular_code_gen/account_directory/account_module.py](../../../../../../3_crew/community_contributions/modular_code_gen/account_directory/account_module.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `AccountManager` | class |
| 2 | `create_account` | function |
| 5 | `deposit` | function |
| 8 | `withdraw` | function |

**Read in this order:**

- Define class `AccountManager`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/account_directory/accounts.py](../../../../../../3_crew/community_contributions/modular_code_gen/account_directory/accounts.py)

**Imports:**

- `account_module` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `portfolio_module` — Local or third-party dependency; follow its definition/import before execution.
- `share_price_module` — Local or third-party dependency; follow its definition/import before execution.
- `transaction_module` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `create_account` | function |
| 15 | `deposit` | function |
| 18 | `withdraw` | function |
| 21 | `buy_shares` | function |
| 24 | `sell_shares` | function |
| 27 | `get_transactions` | function |
| 30 | `calculate_portfolio_value` | function |
| 33 | `get_share_price` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `account_module`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `transaction_module`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `portfolio_module`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `share_price_module`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `account_manager`. Re-executing this cell updates kernel state.
- Store expression result in `transaction_manager`. Re-executing this cell updates kernel state.
- Store expression result in `portfolio_manager`. Re-executing this cell updates kernel state.
- Store expression result in `share_price_fetcher`. Re-executing this cell updates kernel state.
- Define `create_account(user_id)`. Its body runs when called.
- Define `deposit(user_id, amount)`. Its body runs when called.
- Define `withdraw(user_id, amount)`. Its body runs when called.
- Define `buy_shares(user_id, symbol, quantity)`. Its body runs when called.
- Define `sell_shares(user_id, symbol, quantity)`. Its body runs when called.
- Define `get_transactions(user_id)`. Its body runs when called.
- Define `calculate_portfolio_value(user_id)`. Its body runs when called.
- Define `get_share_price(symbol)`. Its body runs when called.
- Store expression result in `app`. Re-executing this cell updates kernel state.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.
- `transaction_manager.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `transaction_manager.sell_shares`: Change the simulated account holdings, cash, and transaction records.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/account_directory/portfolio_module.py](../../../../../../3_crew/community_contributions/modular_code_gen/account_directory/portfolio_module.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `PortfolioManager` | class |
| 2 | `calculate_value` | function |
| 5 | `calculate_profit_loss` | function |
| 8 | `get_holdings` | function |

**Read in this order:**

- Define class `PortfolioManager`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/account_directory/share_price_module.py](../../../../../../3_crew/community_contributions/modular_code_gen/account_directory/share_price_module.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `SharePriceFetcher` | class |
| 2 | `get_share_price` | function |

**Read in this order:**

- Define class `SharePriceFetcher`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/account_directory/transaction_module.py](../../../../../../3_crew/community_contributions/modular_code_gen/account_directory/transaction_module.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `TransactionManager` | class |
| 2 | `buy_shares` | function |
| 5 | `sell_shares` | function |
| 8 | `get_transactions` | function |

**Read in this order:**

- Define class `TransactionManager`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/__init__.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/crew.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `modular_code_gen.tools.file_saver` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.
- `yaml` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `ModuleDescriptions` | class |
| 17 | `MainAPIFile` | class |
| 23 | `ModuleSourceAndTestFiles` | class |
| 41 | `EngineeringMembers` | class |
| 48 | `engineering_lead` | function |
| 55 | `backend_engineer` | function |
| 66 | `frontend_engineer` | function |
| 79 | `file_writer` | function |
| 87 | `design_task` | function |
| 94 | `code_task` | function |
| 101 | `writer_task_back` | function |
| 105 | `frontend_task` | function |
| 112 | `writer_task_front` | function |
| 116 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modular_code_gen.tools.file_saver`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `yaml`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ModuleDescriptions` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MainAPIFile` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ModuleSourceAndTestFiles` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EngineeringMembers`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/main.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `modular_code_gen.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 33 | `run` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `modular_code_gen.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `requirements`. Re-executing this cell updates kernel state.
- Store expression result in `main_file_path`. Re-executing this cell updates kernel state.
- Store expression result in `API_tool`. Re-executing this cell updates kernel state.
- Store expression result in `project_directory`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `EngineeringMembers(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/__init__.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/custom_tool.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/custom_tool.py)

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

### [3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/file_saver.py](../../../../../../3_crew/community_contributions/modular_code_gen/src/modular_code_gen/tools/file_saver.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `SaveFilesInput` | class |
| 15 | `SaveFilesTool` | class |
| 22 | `_run` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SaveFilesInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SaveFilesTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `saved_paths.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
