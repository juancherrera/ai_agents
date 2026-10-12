# 02 medguard

Source: [1_foundations/community_contributions/2_medguard_debate/02_medguard.ipynb](../../../../../../../1_foundations/community_contributions/2_medguard_debate/02_medguard.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Messages → Client, request, and response are separate objects → Await vs. concurrent work → Evaluate, give feedback, and bound revision

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Follow the source in order

### Cell 1: 💊 MedGuard Debate

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Store expression result in `MODEL_AGENT`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL_JUDGE`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Define class `MedicalKnowledgeBase`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Define class `DebateAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Define class `JudgeAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define class `AgenticDebateCoordinator`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Define `main()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.sleep`: Pause. asyncio.sleep yields the loop; time.sleep blocks the current thread.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 4 | `MedicalKnowledgeBase` | class |
| 4 | `get_cautious_evidence` | function |
| 4 | `get_pragmatic_evidence` | function |
| 4 | `get_risk_benefit_evidence` | function |
| 5 | `DebateAgent` | class |
| 5 | `__init__` | function |
| 5 | `build_argument` | async function |
| 5 | `_create_debate_prompt` | function |
| 5 | `_format_evidence` | function |
| 5 | `_call_openai` | async function |
| 6 | `JudgeAgent` | class |
| 6 | `__init__` | function |
| 6 | `judge_debate` | async function |
| 6 | `_create_judge_prompt` | function |
| 6 | `_call_openai_judge` | function |
| 7 | `AgenticDebateCoordinator` | class |
| 7 | `__init__` | function |
| 7 | `conduct_debate` | async function |
| 7 | `display_debate` | function |
| 8 | `main` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
