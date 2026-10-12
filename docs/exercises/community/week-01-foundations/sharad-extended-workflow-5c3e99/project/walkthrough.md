# sharad extended workflow: project walkthrough

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

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Configuration and supporting files

- [1_foundations/community_contributions/sharad_extended_workflow/readme.md](../../../../../../1_foundations/community_contributions/sharad_extended_workflow/readme.md): LLM Router & Evaluator-Optimizer Workflow. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/sharad_extended_workflow/main.py](../../../../../../1_foundations/community_contributions/sharad_extended_workflow/main.py)

**Imports:**

- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `EvaluationResult` | class |
| 11 | `router_llm` | function |
| 29 | `math_llm` | function |
| 41 | `translate_llm` | function |
| 53 | `summarize_llm` | function |
| 65 | `evaluator_llm` | function |
| 84 | `generate_solution` | function |
| 99 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Define class `EvaluationResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `router_llm(user_input)`. Its body runs when called.
- Define `math_llm(user_input)`. Its body runs when called.
- Define `translate_llm(user_input)`. Its body runs when called.
- Define `summarize_llm(user_input)`. Its body runs when called.
- Define `evaluator_llm(task, user_input, solution)`. Its body runs when called.
- Define `generate_solution(task, user_input, feedback)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
