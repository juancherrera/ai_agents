# 03 pagebotai

Source: [1_foundations/community_contributions/3_pagebotai_crawler/03_pagebotai.ipynb](../../../../../../../1_foundations/community_contributions/3_pagebotai_crawler/03_pagebotai.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Messages → Client, request, and response are separate objects → An agent loop must make progress and stop → Extracted documents become prompt context → Generated code is an output before it is execution → Request, status, and parsed body

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: 🌐 PageBotAI - Minimal Notebook Version

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: ============= Import libraries =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `urllib.parse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `yaml`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `bs4`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pocketflow`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: ============= CONFIGURATION =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Store expression result in `LLM_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_TEMPERATURE`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_ITERATIONS`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_URLS_PER_ITERATION`. Re-executing this cell updates kernel state.
- Store expression result in `CONTENT_MAX_CHARS`. Re-executing this cell updates kernel state.
- Store expression result in `MAX_LINKS_PER_PAGE`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: ============= HELPER FUNCTIONS =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Define `is_valid_url(url, allowed_domains)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Define `filter_valid_urls(urls, allowed_domains)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Define `call_llm(prompt)`. Its body runs when called.
- `openai.OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: ============= POCKETFLOW NODES =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 12: code

- Define class `CrawlAndExtract` using `BatchNode`. Separate the class definition, constructing an instance, and calling its methods.
- `shared.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `urls_to_crawl.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `links.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define class `AgentDecision` using `Node`. Separate the class definition, constructing an instance, and calling its methods.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `shared.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `unvisited_display.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `exec_res.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Define class `DraftAnswer` using `Node`. Separate the class definition, constructing an instance, and calling its methods.
- `shared.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: ============= MAIN WORKFLOW =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 16: code

- Define `create_support_bot_flow()`. Its body runs when called.
- Define `run_chatbot(question, target_urls, instruction)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `flow.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `shared.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: ============= USAGE =============

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Store expression result in `openai.api_key`. Re-executing this cell updates kernel state.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 8 | `is_valid_url` | function |
| 9 | `filter_valid_urls` | function |
| 10 | `call_llm` | function |
| 12 | `CrawlAndExtract` | class |
| 12 | `prep` | function |
| 12 | `exec` | function |
| 12 | `exec_fallback` | function |
| 12 | `post` | function |
| 13 | `AgentDecision` | class |
| 13 | `prep` | function |
| 13 | `exec` | function |
| 13 | `exec_fallback` | function |
| 13 | `post` | function |
| 14 | `DraftAnswer` | class |
| 14 | `prep` | function |
| 14 | `exec` | function |
| 14 | `exec_fallback` | function |
| 14 | `post` | function |
| 16 | `create_support_bot_flow` | function |
| 16 | `run_chatbot` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
