# 1 lab2 Routing Workflow

Source: [1_foundations/community_contributions/1_lab2_Routing_Workflow.ipynb](../../../../../../../1_foundations/community_contributions/1_lab2_Routing_Workflow.ipynb). Practice: [commented notebook](practice.ipynb).

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

### Cell 1: Judging and Routing — Optimizing Resource Usage by Evaluating Problem Complexity

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: Imports and Environment Setup

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Creating Models

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 9: code

- Store dictionary in `model_specs`. Re-executing this cell updates kernel state.
- Define `create_model(model_name)`. Its body runs when called.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store expression result in `orchestrator_model`. Re-executing this cell updates kernel state.
- Store expression result in `generator`. Re-executing this cell updates kernel state.
- Store expression result in `router`. Re-executing this cell updates kernel state.
- Store expression result in `qa_models`. Re-executing this cell updates kernel state.
- Store expression result in `judges`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: Orchestrator-Worker Workflow

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Store expression result in `request`. Re-executing this cell updates kernel state.
- Append/update expression result in `request`. Re-executing this cell updates kernel state.
- Append/update expression result in `request`. Re-executing this cell updates kernel state.
- Store list in `messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Store expression result in `response`. Re-executing this cell updates kernel state.
- Store expression result in `eval_question`. Re-executing this cell updates kernel state.
- `generator.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: Task Parallelization

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Store list in `question`. Re-executing this cell updates kernel state.
- Store list in `answers`. Re-executing this cell updates kernel state.
- Store list in `competitors`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `model.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `competitors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Store expression result in `report`. Re-executing this cell updates kernel state.
- Append/update expression result in `report`. Re-executing this cell updates kernel state.
- `display`: Render a value inside a notebook.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: Synthetizer/Judge

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 22: code

- Store expression result in `together`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 23: code

- Store formatted string in `judge_prompt`. Re-executing this cell updates kernel state.
- Store list in `judge_messages`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 24: code

- Make available: `collections`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `N`. Re-executing this cell updates kernel state.
- Store expression result in `scores`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `judge.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 25: code

- Store expression result in `sorted_indices`. Re-executing this cell updates kernel state.
- Store expression result in `ranked_model_names`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 26: Routing Workflow

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 27: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 28: code

- Define `classify_question_complexity(question, routing_agent, routing_model)`. Its body runs when called.
- Define `route_question_to_model(question, models_by_rank, classifier_model, model_name)`. Its body runs when called.
- `routing_agent.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 29: code

- Store list in `difficulty_prompts`. Re-executing this cell updates kernel state.
- Define `generate_question(level, generator, generator_model)`. Its body runs when called.
- `generator.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 30: Testing Routing Workflow

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 31: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 32: code

- Define `test_generation_routing(level)`. Its body runs when called.
- `.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `display`: Render a value inside a notebook.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 33: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 34: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 35: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 36: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 37: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 9 | `create_model` | function |
| 28 | `classify_question_complexity` | function |
| 28 | `route_question_to_model` | function |
| 29 | `generate_question` | function |
| 32 | `test_generation_routing` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
