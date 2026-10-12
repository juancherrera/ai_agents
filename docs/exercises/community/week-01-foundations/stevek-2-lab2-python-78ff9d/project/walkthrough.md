# stevek 2 lab2 python: project walkthrough

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

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

## Configuration and supporting files

- [1_foundations/community_contributions/stevek_2_lab2_python/README.md](../../../../../../1_foundations/community_contributions/stevek_2_lab2_python/README.md): Multi-Model Evaluator (2_lab2.py). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/stevek_2_lab2_python/2_lab2.py](../../../../../../1_foundations/community_contributions/stevek_2_lab2_python/2_lab2.py)

**Imports:**

- `anthropic` — Native Claude client.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `_extract_text` | function |
| 71 | `call_openai_model` | function |
| 79 | `call_anthropic_model` | function |
| 100 | `call_gemini_model` | function |
| 114 | `call_deepseek_model` | function |
| 128 | `call_groq_model` | function |
| 142 | `call_ollama_model` | function |
| 176 | `generate_challenge_question` | function |
| 229 | `call_competitor` | function |
| 250 | `collect_competitor_answers` | function |
| 283 | `build_judge_prompt` | function |
| 321 | `judge_responses` | function |
| 337 | `print_rankings` | function |
| 358 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OPENAI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `ANTHROPIC_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `GEMINI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `DEEPSEEK_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `GROQ_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `OLLAMA_BASE_URL`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `openai_client`. Re-executing this cell updates kernel state.
- Define `_extract_text(response, provider)`. Its body runs when called.
- Define `call_openai_model(model, prompt)`. Its body runs when called.
- Define `call_anthropic_model(model, prompt)`. Its body runs when called.
- Define `call_gemini_model(model, prompt)`. Its body runs when called.
- Define `call_deepseek_model(model, prompt)`. Its body runs when called.
- Define `call_groq_model(model, prompt)`. Its body runs when called.
- Define `call_ollama_model(model, prompt)`. Its body runs when called.
- Store expression result in `QUESTION_GENERATOR_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `GENERATOR_SYSTEM_PROMPT`. Re-executing this cell updates kernel state.
- Define `generate_challenge_question()`. Its body runs when called.
- Store list in `COMPETITORS`. Re-executing this cell updates kernel state.
- Define `call_competitor(provider, model, prompt)`. Its body runs when called.
- Define `collect_competitor_answers(question)`. Its body runs when called.
- Define `build_judge_prompt(question, responses)`. Its body runs when called.
- Store expression result in `JUDGE_MODEL`. Re-executing this cell updates kernel state.
- Define `judge_responses(question, responses)`. Its body runs when called.
- Define `print_rankings(judge_result, responses)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai_client.responses.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `client.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `pieces.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `all_answers.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `index_to_response.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `competitor.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
