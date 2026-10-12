# 1 lab2 Kaushik Parallelization

Source: [1_foundations/community_contributions/1_lab2_Kaushik_Parallelization.ipynb](../../../../../../../1_foundations/community_contributions/1_lab2_Kaushik_Parallelization.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → Evaluate, give feedback, and bound revision

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

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Follow the source in order

### Cell 1: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: Refresh dot env

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `open_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: Create initial query to get challange reccomendation

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store expression result in `query`. Re-executing this cell updates kernel state.
- Append/update expression result in `query`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: Call openai gpt-4o-mini

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `challange`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store list in `competitors`. Re-executing this cell updates kernel state.
- Store list in `answers`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: Create messages with the challange query

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Define `gpt_mini_processor()`. Its body runs when called.
- Define `gemini_processor()`. Its body runs when called.
- Define `llama_processor()`. Its body runs when called.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `gemini.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `ollama.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: Paraller execution of LLM calls

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 19: code

- Store expression result in `thread1`. Re-executing this cell updates kernel state.
- Store expression result in `thread2`. Re-executing this cell updates kernel state.
- Store expression result in `thread3`. Re-executing this cell updates kernel state.
- `thread1.start`: Start the receiver runtime/server/resource.
- `thread2.start`: Start the receiver runtime/server/resource.
- `thread3.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Store expression result in `together`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 24: Prompt to judge the LLM results

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 25: code

- Store formatted string in `to_judge`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 26: code

- Store list in `to_judge_message`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 27: Execute o3-mini to analyze the LLM results

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 28: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `result`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 29: code

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
| 17 | `gpt_mini_processor` | function |
| 17 | `gemini_processor` | function |
| 17 | `llama_processor` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
