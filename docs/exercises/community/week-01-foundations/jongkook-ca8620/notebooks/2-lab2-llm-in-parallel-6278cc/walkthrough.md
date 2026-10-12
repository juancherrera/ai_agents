# 2 lab2 llm in parallel

Source: [1_foundations/community_contributions/jongkook/2_lab2-llm_in_parallel.ipynb](../../../../../../../1_foundations/community_contributions/jongkook/2_lab2-llm_in_parallel.ipynb). Practice: [commented notebook](practice.ipynb).

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
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `challenge_question_prompt`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Define `challenge_question(challenge_question_prompt)`. Its body runs when called.
- `display`: Render a value inside a notebook.
- `OpenAI(...).chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store list in `models`. Re-executing this cell updates kernel state.
- Store list in `api_urls`. Re-executing this cell updates kernel state.
- Store list in `api_keys`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store list in `answers`. Re-executing this cell updates kernel state.
- Define `answer_challenge_question(model, url, api_key, challenge_question)`. Its body runs when called.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `OpenAI(...).chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Make available: `threading`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `ask_question_to_llm(challenge_question)`. Its body runs when called.
- `thread.start`: Start the receiver runtime/server/resource.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Make available: `dis`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `judge_llms(challenge_question_prompt, answers)`. Its body runs when called.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `display`: Render a value inside a notebook.
- `OpenAI(...).chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `challenge_question`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 3 | `challenge_question` | function |
| 5 | `answer_challenge_question` | function |
| 6 | `ask_question_to_llm` | function |
| 7 | `judge_llms` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
