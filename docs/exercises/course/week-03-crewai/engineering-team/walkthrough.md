# Engineering team: design, backend, frontend, tests: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

EngineeringTeam runs design_task → code_task → frontend_task → test_task. Backend/test roles enable Docker code execution.

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

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Configuration and supporting files

- [3_crew/engineering_team/example_output_4o/accounts.py_design.md](../../../../../3_crew/engineering_team/example_output_4o/accounts.py_design.md): Python Module: accounts.py. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/engineering_team/example_output_new/accounts.py_design.md](../../../../../3_crew/engineering_team/example_output_new/accounts.py_design.md): Module: accounts.py. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/engineering_team/pyproject.toml](../../../../../3_crew/engineering_team/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/engineering_team/README.md](../../../../../3_crew/engineering_team/README.md): EngineeringTeam Crew. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/engineering_team/src/engineering_team/config/agents.yaml](../../../../../3_crew/engineering_team/src/engineering_team/config/agents.yaml): configuration sections `engineering_lead`, `backend_engineer`, `frontend_engineer`, `test_engineer`. Match these names with code references and supplied input placeholders.
- [3_crew/engineering_team/src/engineering_team/config/tasks.yaml](../../../../../3_crew/engineering_team/src/engineering_team/config/tasks.yaml): configuration sections `design_task`, `code_task`, `frontend_task`, `test_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/engineering_team/example_output_4o/accounts.py](../../../../../3_crew/engineering_team/example_output_4o/accounts.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `get_share_price` | function |
| 10 | `Account` | class |
| 11 | `__init__` | function |
| 24 | `deposit` | function |
| 52 | `withdraw` | function |
| 76 | `buy_shares` | function |
| 114 | `sell_shares` | function |
| 151 | `get_portfolio_value` | function |
| 168 | `get_profit_or_loss` | function |
| 181 | `get_holdings` | function |
| 190 | `get_transactions` | function |
| 199 | `can_withdraw` | function |
| 211 | `can_buy_shares` | function |
| 229 | `can_sell_shares` | function |

**Read in this order:**

- Define `get_share_price(symbol)`. Its body runs when called.
- Define class `Account`. Separate the class definition, constructing an instance, and calling its methods.
- `prices.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_4o/app.py](../../../../../3_crew/engineering_team/example_output_4o/app.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `create_account` | function |
| 14 | `deposit_funds` | function |
| 21 | `withdraw_funds` | function |
| 28 | `buy_stock` | function |
| 39 | `sell_stock` | function |
| 50 | `get_portfolio` | function |
| 77 | `list_transactions` | function |
| 96 | `check_price` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `account`. Re-executing this cell updates kernel state.
- Define `create_account(deposit_amount)`. Its body runs when called.
- Define `deposit_funds(amount)`. Its body runs when called.
- Define `withdraw_funds(amount)`. Its body runs when called.
- Define `buy_stock(symbol, quantity)`. Its body runs when called.
- Define `sell_stock(symbol, quantity)`. Its body runs when called.
- Define `get_portfolio()`. Its body runs when called.
- Define `list_transactions()`. Its body runs when called.
- Define `check_price(symbol)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `account.sell_shares`: Change the simulated account holdings, cash, and transaction records.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_4o/test_accounts.py](../../../../../3_crew/engineering_team/example_output_4o/test_accounts.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `unittest` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `TestGetSharePrice` | class |
| 22 | `TestAccount` | class |
| 7 | `test_known_symbols` | function |
| 15 | `test_unknown_symbol` | function |
| 25 | `setUp` | function |
| 30 | `test_init` | function |
| 38 | `test_deposit_valid` | function |
| 56 | `test_deposit_invalid` | function |
| 68 | `test_withdraw_valid` | function |
| 78 | `test_withdraw_insufficient_funds` | function |
| 86 | `test_withdraw_negative_amount` | function |
| 94 | `test_buy_shares_valid` | function |
| 114 | `test_buy_shares_insufficient_funds` | function |
| 125 | `test_buy_shares_invalid_quantity` | function |
| 140 | `test_sell_shares_valid` | function |
| 159 | `test_sell_shares_insufficient_shares` | function |
| 170 | `test_sell_shares_invalid_quantity` | function |
| 184 | `test_get_portfolio_value` | function |
| 198 | `test_get_profit_or_loss` | function |
| 226 | `test_get_holdings` | function |
| 241 | `test_get_transactions` | function |
| 256 | `test_can_withdraw` | function |
| 269 | `test_can_buy_shares` | function |
| 290 | `test_can_sell_shares` | function |
| 215 | `higher_prices` | function |

**Read in this order:**

- Make available: `unittest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TestGetSharePrice` using `unittest.TestCase`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TestAccount` using `unittest.TestCase`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `self.account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `self.account.sell_shares`: Change the simulated account holdings, cash, and transaction records.
- `account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `prices.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_mini/accounts.py](../../../../../3_crew/engineering_team/example_output_mini/accounts.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `Account` | class |
| 123 | `get_share_price` | function |
| 4 | `__init__` | function |
| 17 | `deposit` | function |
| 28 | `withdraw` | function |
| 42 | `buy_shares` | function |
| 65 | `sell_shares` | function |
| 87 | `portfolio_value` | function |
| 98 | `profit_or_loss` | function |
| 106 | `report_holdings` | function |
| 114 | `report_transactions` | function |

**Read in this order:**

- Define class `Account`. Separate the class definition, constructing an instance, and calling its methods.
- Define `get_share_price(symbol)`. Its body runs when called.
- `mock_prices.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_mini/app.py](../../../../../3_crew/engineering_team/example_output_mini/app.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `create_account` | function |
| 12 | `deposit_funds` | function |
| 16 | `withdraw_funds` | function |
| 23 | `buy_shares` | function |
| 30 | `sell_shares` | function |
| 37 | `view_portfolio` | function |
| 40 | `view_profit_or_loss` | function |
| 43 | `view_transactions` | function |
| 46 | `total_portfolio_value` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `account`. Re-executing this cell updates kernel state.
- Define `create_account(username, initial_deposit)`. Its body runs when called.
- Define `deposit_funds(amount)`. Its body runs when called.
- Define `withdraw_funds(amount)`. Its body runs when called.
- Define `buy_shares(symbol, quantity)`. Its body runs when called.
- Define `sell_shares(symbol, quantity)`. Its body runs when called.
- Define `view_portfolio()`. Its body runs when called.
- Define `view_profit_or_loss()`. Its body runs when called.
- Define `view_transactions()`. Its body runs when called.
- Define `total_portfolio_value()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `app.launch`: Start the UI/server; defining the UI alone does not start it.
- `account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `account.sell_shares`: Change the simulated account holdings, cash, and transaction records.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_new/accounts.py](../../../../../3_crew/engineering_team/example_output_new/accounts.py)

| Source line | Definition | Kind |
| --- | --- | --- |
| 1 | `get_share_price` | function |
| 19 | `Account` | class |
| 26 | `__init__` | function |
| 46 | `deposit_funds` | function |
| 61 | `withdraw_funds` | function |
| 84 | `buy_shares` | function |
| 120 | `sell_shares` | function |
| 157 | `calculate_portfolio_value` | function |
| 170 | `calculate_profit_or_loss` | function |
| 178 | `get_holdings` | function |
| 186 | `get_transactions` | function |
| 194 | `get_report` | function |

**Read in this order:**

- Define `get_share_price(symbol)`. Its body runs when called.
- Define class `Account`. Separate the class definition, constructing an instance, and calling its methods.
- `prices.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_new/app.py](../../../../../3_crew/engineering_team/example_output_new/app.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `create_account` | function |
| 23 | `deposit` | function |
| 38 | `withdraw` | function |
| 55 | `buy_shares` | function |
| 78 | `sell_shares` | function |
| 97 | `get_portfolio_value` | function |
| 104 | `get_profit_loss` | function |
| 114 | `get_holdings` | function |
| 130 | `get_transactions` | function |
| 151 | `get_account_info` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `account`. Re-executing this cell updates kernel state.
- Define `create_account(user_id, initial_deposit)`. Its body runs when called.
- Define `deposit(amount)`. Its body runs when called.
- Define `withdraw(amount)`. Its body runs when called.
- Define `buy_shares(symbol, quantity)`. Its body runs when called.
- Define `sell_shares(symbol, quantity)`. Its body runs when called.
- Define `get_portfolio_value()`. Its body runs when called.
- Define `get_profit_loss()`. Its body runs when called.
- Define `get_holdings()`. Its body runs when called.
- Define `get_transactions()`. Its body runs when called.
- Define `get_account_info()`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `account.sell_shares`: Change the simulated account holdings, cash, and transaction records.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/example_output_new/test_accounts.py](../../../../../3_crew/engineering_team/example_output_new/test_accounts.py)

**Imports:**

- `accounts` — Local or third-party dependency; follow its definition/import before execution.
- `unittest` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `TestGetSharePrice` | class |
| 13 | `TestAccount` | class |
| 5 | `test_valid_symbols` | function |
| 10 | `test_invalid_symbol` | function |
| 14 | `setUp` | function |
| 17 | `test_initialization` | function |
| 25 | `test_deposit_funds` | function |
| 31 | `test_withdraw_funds_success` | function |
| 38 | `test_withdraw_funds_failure` | function |
| 44 | `test_buy_shares_success` | function |
| 52 | `test_buy_shares_failure` | function |
| 59 | `test_sell_shares_success` | function |
| 67 | `test_sell_shares_failure` | function |
| 74 | `test_calculate_portfolio_value` | function |
| 80 | `test_calculate_profit_or_loss` | function |
| 86 | `test_get_holdings` | function |
| 94 | `test_get_transactions` | function |
| 102 | `test_get_report` | function |

**Read in this order:**

- Make available: `unittest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `accounts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TestGetSharePrice` using `unittest.TestCase`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TestAccount` using `unittest.TestCase`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `self.account.buy_shares`: Change the simulated account holdings, cash, and transaction records.
- `self.account.sell_shares`: Change the simulated account holdings, cash, and transaction records.
- `transactions.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/src/engineering_team/__init__.py](../../../../../3_crew/engineering_team/src/engineering_team/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/src/engineering_team/crew.py](../../../../../3_crew/engineering_team/src/engineering_team/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `EngineeringTeam` | class |
| 14 | `engineering_lead` | function |
| 21 | `backend_engineer` | function |
| 32 | `frontend_engineer` | function |
| 39 | `test_engineer` | function |
| 50 | `design_task` | function |
| 56 | `code_task` | function |
| 62 | `frontend_task` | function |
| 68 | `test_task` | function |
| 74 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `EngineeringTeam`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/src/engineering_team/main.py](../../../../../3_crew/engineering_team/src/engineering_team/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `engineering_team.crew` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 30 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `engineering_team.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `requirements`. Re-executing this cell updates kernel state.
- Store expression result in `module_name`. Re-executing this cell updates kernel state.
- Store expression result in `class_name`. Re-executing this cell updates kernel state.
- Define `run()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `EngineeringTeam(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/src/engineering_team/tools/__init__.py](../../../../../3_crew/engineering_team/src/engineering_team/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/engineering_team/src/engineering_team/tools/custom_tool.py](../../../../../3_crew/engineering_team/src/engineering_team/tools/custom_tool.py)

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

Use [.environment/.env configuration](../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
