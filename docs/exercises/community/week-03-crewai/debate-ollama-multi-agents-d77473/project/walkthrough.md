# debate ollama multi agents: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

## Configuration and supporting files

- [3_crew/community_contributions/debate_ollama_multi_agents/agents.yaml](../../../../../../3_crew/community_contributions/debate_ollama_multi_agents/agents.yaml): configuration sections `debater`, `opponent`, `judge`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/debate_ollama_multi_agents/tasks.yaml](../../../../../../3_crew/community_contributions/debate_ollama_multi_agents/tasks.yaml): configuration sections `propose`, `opponent`, `decide`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/debate_ollama_multi_agents/crew.py](../../../../../../3_crew/community_contributions/debate_ollama_multi_agents/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `Debate` | class |
| 19 | `debater` | function |
| 27 | `opponent` | function |
| 35 | `judge` | function |
| 43 | `propose` | function |
| 49 | `oppose` | function |
| 55 | `decide` | function |
| 62 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `llm_debater`. Re-executing this cell updates kernel state.
- Store expression result in `llm_opposer`. Re-executing this cell updates kernel state.
- Store expression result in `llm_judge`. Re-executing this cell updates kernel state.
- Define class `Debate`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
