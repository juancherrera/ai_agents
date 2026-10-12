# Multi Model Resume–JD Match Analyzer: project walkthrough

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

- [1_foundations/community_contributions/Multi-Model-Resume–JD-Match-Analyzer/README.md](../../../../../../1_foundations/community_contributions/Multi-Model-Resume%E2%80%93JD-Match-Analyzer/README.md): 🧠 Resume-Job Match Application (LLM-Powered). Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/Multi-Model-Resume–JD-Match-Analyzer/multi_file_ingestion.py](../../../../../../1_foundations/community_contributions/Multi-Model-Resume%E2%80%93JD-Match-Analyzer/multi_file_ingestion.py)

**Imports:**

- `langchain.document_loaders` — Model/tool wrappers.
- `os` — Process configuration and operating-system paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `load_and_split_resume` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.document_loaders`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `load_and_split_resume(file_path)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/Multi-Model-Resume–JD-Match-Analyzer/resume_agent.py](../../../../../../1_foundations/community_contributions/Multi-Model-Resume%E2%80%93JD-Match-Analyzer/resume_agent.py)

**Imports:**

- `anthropic` — Native Claude client.
- `dotenv` — Load explicit environment-file configuration.
- `io` — Local or third-party dependency; follow its definition/import before execution.
- `multi_file_ingestion` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pandas` — Tabular data.
- `pdfplumber` — Local or third-party dependency; follow its definition/import before execution.
- `streamlit` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 54 | `extract_text` | function |
| 62 | `extract_candidate_name` | function |
| 90 | `build_prompt` | function |
| 110 | `get_openai_match` | function |
| 127 | `get_anthropic_match` | function |
| 147 | `get_google_match` | function |
| 161 | `get_groq_match` | function |
| 175 | `get_deepseek_match` | function |
| 238 | `render_custom_table` | function |

**Read in this order:**

- Make available: `streamlit`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `anthropic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pdfplumber`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `io`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `multi_file_ingestion`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `anthropic_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `google_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `groq_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `deepseek_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `resume_file`. Re-executing this cell updates kernel state.
- Store expression result in `jd_file`. Re-executing this cell updates kernel state.
- Define `extract_text(file)`. Its body runs when called.
- Define `extract_candidate_name(resume_text)`. Its body runs when called.
- Define `build_prompt(resume_text, jd_text)`. Its body runs when called.
- Define `get_openai_match(prompt)`. Its body runs when called.
- Define `get_anthropic_match(prompt)`. Its body runs when called.
- Define `get_google_match(prompt)`. Its body runs when called.
- Define `get_groq_match(prompt)`. Its body runs when called.
- Define `get_deepseek_match(prompt)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `StringIO(...).read`: Read content from a file-like object. Check the receiver and its lifetime.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `Anthropic`: Construct the native Claude client; its response shape differs from compatible chat clients.
- `claude.messages.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `gemini.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `groq.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `deepseek.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `pdfplumber.open`: Open a file or resource. Inspect the mode and path before running it.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `file.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
