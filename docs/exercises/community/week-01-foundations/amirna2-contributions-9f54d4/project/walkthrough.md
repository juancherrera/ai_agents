# amirna2 contributions: project walkthrough

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

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/amirna2_contributions/personal-ai/docs/prompt-refactoring-plan.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/docs/prompt-refactoring-plan.md): Prompt Management Refactoring Plan. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_base.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_base.md): CONTEXT:. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_init.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_init.md): CRITICAL INSTRUCTIONS AND RULES:. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_rerun.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/chat_rerun.md): Previous answer rejected. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/evaluator.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/evaluator.md): CONTEXT AVAILABLE TO AGENT:. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/evaluator_with_github_context.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/evaluator_with_github_context.md): GitHub Tool Results (VALID CONTEXT):. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/job_match_analysis.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/prompts/job_match_analysis.md): Explanation, context, or transition. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/pyproject.toml](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/README.md](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/README.md): AI Career Assistant. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/amirna2_contributions/personal-ai/requirements.txt](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/career_chatbot.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/career_chatbot.py)

**Imports:**

- `base64` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `promptkit` — Local or third-party dependency; follow its definition/import before execution.
- `pypdf` — Embedded PDF-text extraction.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `typer` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 37 | `NotificationService` | class |
| 72 | `WebSearchService` | class |
| 234 | `DocumentLoader` | class |
| 280 | `Evaluator` | class |
| 542 | `ToolRegistry` | class |
| 757 | `CareerChatbot` | class |
| 967 | `main` | function |
| 40 | `__init__` | function |
| 51 | `send` | function |
| 75 | `__init__` | function |
| 92 | `search_github_repos` | function |
| 181 | `get_repo_details` | function |
| 238 | `load_pdf` | function |
| 268 | `load_text` | function |
| 283 | `__init__` | function |
| 294 | `_create_evaluator_prompt` | function |
| 330 | `_create_user_prompt` | function |
| 351 | `rerun` | function |
| 374 | `_create_base_system_prompt` | function |
| 382 | `evaluate_structured` | function |
| 469 | `_external_tools_used` | function |
| 485 | `_is_github_context` | function |
| 489 | `_is_job_matching_context` | function |
| 507 | `_create_evaluator_prompt_with_github` | function |
| 525 | `_extract_github_context_from_history` | function |
| 545 | `__init__` | function |
| 555 | `_create_tool_definitions` | function |
| 666 | `record_user_details` | function |
| 676 | `evaluate_job_match` | function |
| 719 | `handle_tool_calls` | function |
| 760 | `__init__` | function |
| 780 | `_load_context` | function |
| 789 | `_create_system_prompt` | function |
| 809 | `chat` | function |
| 863 | `_generate_response_with_tools` | function |
| 927 | `create_initial_greeting` | function |
| 937 | `launch_interface` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `promptkit`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `NotificationService`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `WebSearchService`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `DocumentLoader`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Evaluator`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ToolRegistry`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CareerChatbot`. Separate the class definition, constructing an instance, and calling its methods.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `self.evaluator_client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `interface.launch`: Start the UI/server; defining the UI alone does not start it.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `self.session.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `tools.extend`: Add every item from an iterable to a list.
- `self.openai_client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `repo.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `formatted_repos.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `message.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `pending_notifications.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `arguments.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.dumps`: Convert Python data to JSON text.
- `all_pending_notifications.extend`: Add every item from an iterable to a list.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `filtered.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.openai_client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/models/__init__.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/models/__init__.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.evaluation` — Local or third-party dependency; follow its definition/import before execution.
- `.job_match` — Local or third-party dependency; follow its definition/import before execution.
- `.responses` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.evaluation`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.responses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.job_match`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/models/config.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/models/config.py)

**Imports:**

- `dataclasses` — Simple typed records.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `ChatbotConfig` | class |

**Read in this order:**

- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ChatbotConfig`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/models/evaluation.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/models/evaluation.py)

**Imports:**

- `pydantic` — Validated structured objects.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `Evaluation` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Evaluation` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/models/job_match.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/models/job_match.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `SkillAssessment` | class |
| 12 | `JobMatchResult` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SkillAssessment` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `JobMatchResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/models/responses.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/models/responses.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `StructuredResponse` | class |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `StructuredResponse` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/amirna2_contributions/personal-ai/promptkit.py](../../../../../../1_foundations/community_contributions/amirna2_contributions/personal-ai/promptkit.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.
- `re` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `_get` | function |
| 12 | `render` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `_pat`. Re-executing this cell updates kernel state.
- Define `_get(ctx, path)`. Its body runs when called.
- Define `render(path, vars)`. Its body runs when called.
- `re.compile`: Build an executable graph or compiled object; this is separate from running it.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
