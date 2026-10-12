# customer care agents: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

## Configuration and supporting files

- [2_openai/community_contributions/customer_care_agents/README.md](../../../../../../2_openai/community_contributions/customer_care_agents/README.md): Customer Care Agent – Google ADK Project. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [2_openai/community_contributions/customer_care_agents/__init__.py](../../../../../../2_openai/community_contributions/customer_care_agents/__init__.py)

**Imports:**

- `.` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/customer_care_agents/agent.py](../../../../../../2_openai/community_contributions/customer_care_agents/agent.py)

**Imports:**

- `google.adk.agents` — Local or third-party dependency; follow its definition/import before execution.
- `google.adk.agents.sequential_agent` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `pandas` — Tabular data.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.

| Source line | Definition | Kind |
| --- | --- | --- |
| 21 | `generate_unique_perfume_data` | function |
| 66 | `QueryGeneratorOutput` | class |
| 70 | `execute_query` | function |

**Read in this order:**

- Make available: `google.adk.agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `google.adk.agents.sequential_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `brands`. Re-executing this cell updates kernel state.
- Store list in `concentrations`. Re-executing this cell updates kernel state.
- Store list in `genders`. Re-executing this cell updates kernel state.
- Store list in `availability_status`. Re-executing this cell updates kernel state.
- Store list in `fragrance_families`. Re-executing this cell updates kernel state.
- Store list in `seasons`. Re-executing this cell updates kernel state.
- Store expression result in `launch_years`. Re-executing this cell updates kernel state.
- Store list in `adjectives`. Re-executing this cell updates kernel state.
- Store list in `scent_keywords`. Re-executing this cell updates kernel state.
- Define `generate_unique_perfume_data(n)`. Its body runs when called.
- Store expression result in `df`. Re-executing this cell updates kernel state.
- Store expression result in `query_refiner_agent`. Re-executing this cell updates kernel state.
- Define class `QueryGeneratorOutput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `execute_query(query)`. Its body runs when called.
- Store expression result in `query_generator_agent`. Re-executing this cell updates kernel state.
- Store expression result in `query_execution_agent`. Re-executing this cell updates kernel state.
- Store expression result in `data_query_agent`. Re-executing this cell updates kernel state.
- Store expression result in `root_agent`. Re-executing this cell updates kernel state.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Field`: Describe a schema field or constrain allowed values.
- `products.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
