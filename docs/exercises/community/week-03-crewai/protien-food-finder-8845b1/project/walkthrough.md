# protien food finder: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/agents.yaml](../../../../../../3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/agents.yaml): configuration sections `store_locator`, `nutrition_researcher`, `nutrition_validator`, `store_specialist_template`, `recommendation_specialist`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/settings.yaml](../../../../../../3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/settings.yaml): configuration sections `products_per_store`, `agent_behavior`, `stores`, `dietary_validation`, `search_strategy`, `report`, `llm`, `logging`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/tasks.yaml](../../../../../../3_crew/community_contributions/protien_food_finder/src/protien_food_finder/config/tasks.yaml): configuration sections `find_stores`, `store_search_template`, `research_protein_items`, `validate_products`, `create_recommendations`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/protien_food_finder/src/protien_food_finder/crew.py](../../../../../../3_crew/community_contributions/protien_food_finder/src/protien_food_finder/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `crewai_tools` — Crew search and action integrations.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.
- `yaml` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `ProtienFoodFinder` | class |
| 18 | `__init__` | function |
| 31 | `_load_settings` | function |
| 46 | `store_locator` | function |
| 54 | `nutrition_researcher` | function |
| 62 | `nutrition_validator` | function |
| 69 | `recommendation_specialist` | function |
| 75 | `parse_stores_from_output` | function |
| 105 | `create_store_specialist_agent` | function |
| 157 | `create_store_search_task` | function |
| 209 | `find_stores_task` | function |
| 215 | `research_protein_items_task` | function |
| 221 | `validate_products_task` | function |
| 227 | `create_recommendations_task` | function |
| 232 | `build_dynamic_crew` | function |
| 336 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai_tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `yaml`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ProtienFoodFinder`. Separate the class definition, constructing an instance, and calling its methods.
- `SerperDevTool`: Attach hosted search; it needs configured service access.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.
- `initial_crew.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `self.agents_config.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `store_info.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.tasks_config.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `template.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.dynamic_agents.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.settings.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.dynamic_tasks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `stores.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/protien_food_finder/src/protien_food_finder/main.py](../../../../../../3_crew/community_contributions/protien_food_finder/src/protien_food_finder/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `protien_food_finder.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `validate_api_keys` | function |
| 39 | `run` | function |
| 127 | `train` | function |
| 147 | `replay` | function |
| 160 | `test` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `protien_food_finder.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `validate_api_keys()`. Its body runs when called.
- Define `run()`. Its body runs when called.
- Define `train()`. Its body runs when called.
- Define `replay()`. Its body runs when called.
- Define `test()`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `missing_keys.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `dynamic_crew.kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `crew_instance.crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
