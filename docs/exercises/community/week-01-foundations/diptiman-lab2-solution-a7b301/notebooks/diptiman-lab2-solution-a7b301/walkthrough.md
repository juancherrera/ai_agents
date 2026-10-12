# diptiman lab2 solution

Source: [1_foundations/community_contributions/diptiman_lab2_solution.ipynb](../../../../../../../1_foundations/community_contributions/diptiman_lab2_solution.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → An agent loop must make progress and stop → Evaluate, give feedback, and bound revision → Request, status, and parsed body

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: Welcome to the Second Lab - Week 1, Day 3

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Important point - please read

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `perplexity_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Store expression result in `request`. Re-executing this cell updates kernel state.
- Append/update expression result in `request`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `question`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store list in `competitors`. Re-executing this cell updates kernel state.
- Store list in `answers`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: Note - update since the videos

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `claude`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `claude.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `httpx`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `urllib3`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `httpx_client`. Re-executing this cell updates kernel state.
- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `perplexity_client`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `perplexity_client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Store expression result in `gemini`. Re-executing this cell updates kernel state.
- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `gemini.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Store expression result in `deepseek`. Re-executing this cell updates kernel state.
- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `deepseek.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `groq`. Re-executing this cell updates kernel state.
- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `groq.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: For the next cell, we will use Ollama

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: Super important - ignore me at your peril!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 19: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: code

- Store expression result in `ollama`. Re-executing this cell updates kernel state.
- Store expression result in `model_name`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `answer`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `ollama.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 21: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 22: code

- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- Store expression result in `together`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 24: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: blank separator

No execution needed.

### Cell 26: code

- Store formatted string in `judge`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 27: code

- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 28: code

- Store list in `judge_messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 29: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `results`. Re-executing this cell updates kernel state.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 30: code

- Store expression result in `results_dict`. Re-executing this cell updates kernel state.
- Store expression result in `ranks`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 31: Exercise

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 32: Commercial implications

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
