# 1 swap trading prism: project walkthrough

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

### Parameterized SQL and isolated data

SQLite stores records. Parameterized values keep data separate from SQL syntax. We use :memory: so the exercise neither deletes nor changes the source databases.

**Ask yourself:** What happens when you query a city that is not present?

**Practice:** Insert another fictional city and query it. Explain why source code deleting tickets.db should not run just to inspect an import.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/1_swap_trading_prism/pyproject.toml](../../../../../../3_crew/community_contributions/1_swap_trading_prism/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/1_swap_trading_prism/README.md](../../../../../../3_crew/community_contributions/1_swap_trading_prism/README.md): 💹 PRISM - Position Risk Intelligence & Swap Monitor. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/1_swap_trading_prism/src/prism/config/agents.yaml](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/config/agents.yaml): configuration sections `market_data_agent`, `position_manager_agent`, `risk_calculator_agent`, `trading_decision_agent`, `risk_manager_agent`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/1_swap_trading_prism/src/prism/config/tasks.yaml](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/config/tasks.yaml): configuration sections `fetch_market_data_task`, `load_positions_task`, `set_thresholds_task`, `calculate_risk_task`, `make_trading_decision_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/1_swap_trading_prism/src/prism/__init__.py](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/1_swap_trading_prism/src/prism/crew.py](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/crew.py)

**Imports:**

- `.database` — Local or third-party dependency; follow its definition/import before execution.
- `.tools` — Local or third-party dependency; follow its definition/import before execution.
- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 26 | `PrismCrew` | class |
| 127 | `main` | function |
| 33 | `market_data_agent` | function |
| 42 | `position_manager_agent` | function |
| 51 | `risk_calculator_agent` | function |
| 64 | `risk_manager_agent` | function |
| 73 | `trading_decision_agent` | function |
| 82 | `fetch_market_data_task` | function |
| 89 | `load_positions_task` | function |
| 96 | `set_thresholds_task` | function |
| 103 | `calculate_risk_task` | function |
| 110 | `make_trading_decision_task` | function |
| 117 | `crew` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `TENORS`. Re-executing this cell updates kernel state.
- Store expression result in `DEFAULT_CURRENCY`. Re-executing this cell updates kernel state.
- Define class `PrismCrew`. Separate the class definition, constructing an instance, and calling its methods.
- Define `main()`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `PrismCrew(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `SerperDevTool`: Attach hosted search; it needs configured service access.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/1_swap_trading_prism/src/prism/database.py](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/database.py)

**Imports:**

- `os` — Process configuration and operating-system paths.
- `sqlite3` — Local database storage.

| Source line | Definition | Kind |
| --- | --- | --- |
| 48 | `DatabaseConnection` | class |
| 112 | `initialize_database` | function |
| 125 | `seed_positions` | function |
| 51 | `__init__` | function |
| 60 | `connect` | function |
| 70 | `close` | function |
| 75 | `execute_query` | function |
| 99 | `initialize_schema` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sqlite3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SCHEMA_SQL`. Re-executing this cell updates kernel state.
- Define class `DatabaseConnection`. Separate the class definition, constructing an instance, and calling its methods.
- Define `initialize_database()`. Its body runs when called.
- Define `seed_positions()`. Its body runs when called.
- `db.connect`: Open a database or network/server connection; identify the receiver.
- `db.close`: Release the resource; match setup with cleanup.
- `os.remove`: Delete an item/file depending on the receiver. Check the target.
- `sqlite3.connect`: Open a database or network/server connection; identify the receiver.
- `self.conn.close`: Release the resource; match setup with cleanup.
- `cursor.execute`: Execute an operation such as SQL; inspect the statement and its effect.
- `cursor.close`: Release the resource; match setup with cleanup.
- `self.conn.commit`: Persist pending database changes or another transaction.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/1_swap_trading_prism/src/prism/tools.py](../../../../../../3_crew/community_contributions/1_swap_trading_prism/src/prism/tools.py)

**Imports:**

- `.database` — Local or third-party dependency; follow its definition/import before execution.
- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `_calculate_years_to_maturity_internal` | function |
| 28 | `_calculate_swap_pnl_internal` | function |
| 55 | `_check_trading_signal_internal` | function |
| 77 | `_calculate_dynamic_thresholds_internal` | function |
| 101 | `calculate_swap_pnl` | function |
| 119 | `calculate_years_to_maturity` | function |
| 130 | `check_trading_signal` | function |
| 143 | `calculate_dynamic_thresholds` | function |
| 156 | `get_all_positions` | function |
| 175 | `get_position_by_id` | function |
| 191 | `insert_trade_signal` | function |
| 221 | `store_market_rates` | function |
| 255 | `get_latest_market_rate` | function |

**Read in this order:**

- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.database`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_calculate_years_to_maturity_internal(maturity_date)`. Its body runs when called.
- Define `_calculate_swap_pnl_internal(position, current_rate)`. Its body runs when called.
- Define `_check_trading_signal_internal(pnl, threshold_profit, threshold_loss)`. Its body runs when called.
- Define `_calculate_dynamic_thresholds_internal(position, volatility)`. Its body runs when called.
- Define `calculate_swap_pnl(position, current_rate)`. Its body runs when called.
- Define `calculate_years_to_maturity(maturity_date)`. Its body runs when called.
- Define `check_trading_signal(pnl, threshold_profit, threshold_loss)`. Its body runs when called.
- Define `calculate_dynamic_thresholds(position, volatility)`. Its body runs when called.
- Define `get_all_positions()`. Its body runs when called.
- Define `get_position_by_id(position_id)`. Its body runs when called.
- Define `insert_trade_signal(position_id, signal_type, current_pnl, reason, recommended_action)`. Its body runs when called.
- Define `store_market_rates(rates)`. Its body runs when called.
- Define `get_latest_market_rate(tenor, currency)`. Its body runs when called.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `Field`: Describe a schema field or constrain allowed values.
- `db.connect`: Open a database or network/server connection; identify the receiver.
- `db.close`: Release the resource; match setup with cleanup.
- `rate.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
