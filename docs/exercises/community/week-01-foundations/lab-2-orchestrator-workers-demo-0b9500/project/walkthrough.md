# lab 2 orchestrator workers demo: project walkthrough

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

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Configuration and supporting files

- [1_foundations/community_contributions/lab_2_orchestrator_workers_demo/pyproject.toml](../../../../../../1_foundations/community_contributions/lab_2_orchestrator_workers_demo/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [1_foundations/community_contributions/lab_2_orchestrator_workers_demo/README_orchestrator_workers.md](../../../../../../1_foundations/community_contributions/lab_2_orchestrator_workers_demo/README_orchestrator_workers.md): Orchestrator-Workers Workflow Demo. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/lab_2_orchestrator_workers_demo/orchestrator_workers_demo.py](../../../../../../1_foundations/community_contributions/lab_2_orchestrator_workers_demo/orchestrator_workers_demo.py)

**Imports:**

- `anthropic` — Native Claude client.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 27 | `OrchestratorWorkersWorkflow` | class |
| 306 | `compare_workflow_patterns` | function |
| 328 | `main` | function |
| 36 | `__init__` | function |
| 65 | `orchestrate_task_breakdown` | function |
| 120 | `execute_worker_tasks` | function |
| 216 | `synthesize_results` | function |
| 265 | `run_workflow` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `OrchestratorWorkersWorkflow`. Separate the class definition, constructing an instance, and calling its methods.
- Define `compare_workflow_patterns()`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self.openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `worker_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.claude.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `self.gemini.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `self.deepseek.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
