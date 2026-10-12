# 2 lab2 async

Source: [1_foundations/community_contributions/2_lab2_async.ipynb](../../../../../../../1_foundations/community_contributions/2_lab2_async.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → Await vs. concurrent work → Structured output validates shape → Evaluate, give feedback, and bound revision

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

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

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Follow the source in order

### Cell 1: Welcome to the Second Lab - Week 1, Day 3

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `OPENAI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `GOOGLE_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `DEEPSEEK_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `GROQ_API_KEY`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `request`. Re-executing this cell updates kernel state.
- Append/update expression result in `request`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `question`. Re-executing this cell updates kernel state.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Define class `LLMResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store list in `results`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Define `openai_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Define `anthropic_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `claude.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Define `google_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `gemini.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define `deepseek_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `deepseek.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Define `groq_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `groq.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: For the next cell, we will use Ollama

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: Super important - ignore me at your peril!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Define `ollama_answer()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `ollama.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Define `gather_answers()`. Its body runs when called; await/schedule the coroutine to execute async work.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- Store expression result in `together`. Re-executing this cell updates kernel state.
- Store list in `competitors`. Re-executing this cell updates kernel state.
- Store list in `answers`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: code

- Store formatted string in `judge`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- Store list in `judge_messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 24: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `judgement`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: code

- Store expression result in `results_dict`. Re-executing this cell updates kernel state.
- Store expression result in `ranks`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 8 | `LLMResult` | class |
| 10 | `openai_answer` | async function |
| 11 | `anthropic_answer` | async function |
| 12 | `google_answer` | async function |
| 13 | `deepseek_answer` | async function |
| 14 | `groq_answer` | async function |
| 18 | `ollama_answer` | async function |
| 19 | `gather_answers` | async function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
