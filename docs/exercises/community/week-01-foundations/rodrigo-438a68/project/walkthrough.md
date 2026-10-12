# rodrigo: project walkthrough

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

## Module-by-module reading map

### [1_foundations/community_contributions/rodrigo/__init__.py](../../../../../../1_foundations/community_contributions/rodrigo/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/rodrigo/zroddeUtils/__init__.py](../../../../../../1_foundations/community_contributions/rodrigo/zroddeUtils/__init__.py)

**Read in this order:**

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/rodrigo/zroddeUtils/llmModels.py](../../../../../../1_foundations/community_contributions/rodrigo/zroddeUtils/llmModels.py)

**Read in this order:**

- Store expression result in `Gpt_41_nano`. Re-executing this cell updates kernel state.
- Store expression result in `Gpt_41_mini`. Re-executing this cell updates kernel state.
- Store expression result in `Claude_35_haiku`. Re-executing this cell updates kernel state.
- Store expression result in `Claude_37_sonnet`. Re-executing this cell updates kernel state.
- Store expression result in `Gemini_25_Flash_Preview_thinking`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_deepseek_r1`. Re-executing this cell updates kernel state.
- Store expression result in `Gemini_20_flash_001`. Re-executing this cell updates kernel state.
- Store expression result in `free_mistral_Small_31_24B`. Re-executing this cell updates kernel state.
- Store expression result in `free_deepSeek_V3_Base`. Re-executing this cell updates kernel state.
- Store expression result in `free_meta_Llama_4_Maverick`. Re-executing this cell updates kernel state.
- Store expression result in `free_nous_Hermes_3_Mistral_24B`. Re-executing this cell updates kernel state.
- Store expression result in `free_gemini_20_flash_exp`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/rodrigo/zroddeUtils/openRouterUtils.py](../../../../../../1_foundations/community_contributions/rodrigo/zroddeUtils/openRouterUtils.py)

**Imports:**

- `IPython.display` — Notebook display/shell facilities.
- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 25 | `chatWithOpenRouter` | function |
| 55 | `getOpenrouterResponse` | function |
| 85 | `clearChatHistory` | function |

**Read in this order:**

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openrouter_api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `chatHistory`. Re-executing this cell updates kernel state.
- Define `chatWithOpenRouter(model, prompt)`. Its body runs when called.
- Define `getOpenrouterResponse(model, prompt)`. Its body runs when called.
- Define `clearChatHistory()`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `chatHistory.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `llmRequest.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `display`: Render a value inside a notebook.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
