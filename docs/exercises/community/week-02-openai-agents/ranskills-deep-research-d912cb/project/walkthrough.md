# ranskills deep research.py: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### Await vs. concurrent work

Calling an async function creates a coroutine. Await executes/waits for it. gather schedules independent coroutines and returns results in input order. Notebook cells support top-level await.

**Ask yourself:** Will results follow completion order or input order?

**Practice:** Run the two functions with separate awaits. Explain why a dependent stage cannot start until it has its input.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Browser tools have setup, action, and cleanup

A browser toolkit makes actions such as navigation and extraction callable. The practice object imitates that lifetime with fixed HTML; it launches no browser and accesses no website.

**Ask yourself:** Can extraction happen before navigation in this demonstration?

**Practice:** Call extract before navigate and inspect the error. Explain why real Playwright also needs binaries and cleanup.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [2_openai/community_contributions/ranskills-deep-research.py](../../../../../../2_openai/community_contributions/ranskills-deep-research.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `ddgs` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `logging` — Application diagnostics.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `playwright.async_api` — Browser automation with setup/action/cleanup.
- `playwright_stealth` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 60 | `url_to_filename` | function |
| 64 | `generate_safe_filename` | function |
| 80 | `save_report` | function |
| 86 | `save_report_tool` | function |
| 90 | `QueryItem` | class |
| 95 | `QueryItemList` | class |
| 101 | `generate_queries` | async function |
| 122 | `_search_with_serpapi` | async function |
| 136 | `_search_with_duckduckgo` | async function |
| 147 | `perform_online_search` | async function |
| 151 | `aggregate_online_search_results` | async function |
| 162 | `SearchResultItem` | class |
| 168 | `SearchResultItemList` | class |
| 172 | `pick_best_preliminary_search_results` | async function |
| 205 | `fetch_page_content` | async function |
| 247 | `aggregate_page_contents` | async function |
| 262 | `produce_search_report` | async function |
| 291 | `deep_research_pipeline` | async function |

**Read in this order:**

- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `playwright.async_api`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `playwright_stealth`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store dictionary in `LLM_PROVIDERS`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_PROVIDER`. Re-executing this cell updates kernel state.
- Store expression result in `api_key`, `base_url`, `model`. Re-executing this cell updates kernel state.
- Store expression result in `SERPAPI_API_KEY`. Re-executing this cell updates kernel state.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Store expression result in `screenshots_dir`. Re-executing this cell updates kernel state.
- Store expression result in `reports_dir`. Re-executing this cell updates kernel state.
- Define `url_to_filename(url)`. Its body runs when called.
- Define `generate_safe_filename(topic, max_length)`. Its body runs when called.
- Define `save_report(filename, content)`. Its body runs when called.
- Define `save_report_tool(filename, content)`. Its body runs when called.
- Define class `QueryItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `QueryItemList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `generate_queries(topic, num_queries)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `_search_with_serpapi(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `_search_with_duckduckgo(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `perform_online_search(query)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `aggregate_online_search_results(query_result)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define class `SearchResultItem` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SearchResultItemList` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `pick_best_preliminary_search_results(topic, preliminary_search_results, num)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `fetch_page_content(url)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `aggregate_page_contents(search_results)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `produce_search_report(query, context)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Define `deep_research_pipeline(topic, num_generated_queries, num_filtered_search_results, email_address)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Store expression result in `title`. Re-executing this cell updates kernel state.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `AsyncOpenAI`: Construct an asynchronous compatible model client; requests still require await.
- `function_tool`: Build a tool schema from a Python signature/docstring.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Field`: Describe a schema field or constrain allowed values.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `asyncio.gather`: Schedule independent coroutines; returned results follow input order.
- `trace`: Group activity for inspection. Configured tracing may export run data.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `p.chromium.launch`: Start the UI/server; defining the UI alone does not start it.
- `browser.close`: Release the resource; match setup with cleanup.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
