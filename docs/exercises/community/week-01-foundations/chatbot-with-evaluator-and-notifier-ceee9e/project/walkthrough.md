# ChatBot with evaluator and notifier: project walkthrough

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

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/README.md](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/README.md): Smart RAG Chatbot. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/requirements.txt](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/requirements.txt): supporting source/configuration; inspect its inputs and effects before execution.

## Module-by-module reading map

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/app.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/app.py)

**Imports:**

- `controller` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `respond` | function |

**Read in this order:**

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `controller`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `controller`. Re-executing this cell updates kernel state.
- Define `respond(user_msg, history, recorded_emails_state)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `history.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/chat.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/chat.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `ChatGPTLLM` | class |
| 14 | `__init__` | function |
| 19 | `format_context_prompt` | function |
| 30 | `generate_response` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Define class `ChatGPTLLM`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/controller.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/controller.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `evaluator` — Local or third-party dependency; follow its definition/import before execution.
- `langchain.schema` — Model/tool wrappers.
- `langchain_openai` — Compatible model integration for LangChain.
- `os` — Process configuration and operating-system paths.
- `rag` — Local or third-party dependency; follow its definition/import before execution.
- `re` — Local or third-party dependency; follow its definition/import before execution.
- `tools` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `LLM` | function |
| 41 | `_token_set` | function |
| 47 | `Evaluator` | function |
| 100 | `ChatbotController` | class |
| 59 | `jaccard` | function |
| 101 | `__init__` | function |
| 115 | `ingest` | function |
| 118 | `_extract_emails` | function |
| 121 | `_extract_name` | function |
| 134 | `_extract_emails_from_conversation` | function |
| 148 | `_extract_name_from_conversation` | function |
| 164 | `_build_prompt` | function |
| 178 | `_build_conversation_with_history` | function |
| 214 | `_smalltalk_reply` | function |
| 223 | `_is_conversational` | function |
| 244 | `get_response` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain.schema`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rag`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `evaluator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OPENAI_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `DISCLAIMER`. Re-executing this cell updates kernel state.
- Define `LLM(user_input, db_retrieved, history)`. Its body runs when called.
- Define `_token_set(text)`. Its body runs when called.
- Define `Evaluator(user_input, db_retrieved, llm_response, history)`. Its body runs when called.
- Define class `ChatbotController`. Separate the class definition, constructing an instance, and calling its methods.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `feedback_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `msg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `decision.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `re.compile`: Build an executable graph or compiled object; this is separate from running it.
- `fields.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `self.llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/evaluator.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/evaluator.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `GeminiEvaluator` | class |
| 13 | `__init__` | function |
| 19 | `create_evaluation_prompt` | function |
| 34 | `evaluate_response` | function |
| 72 | `evaluate_no_context` | function |
| 48 | `EvaluationResult` | class |
| 92 | `_coerce_conf` | function |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `GEMINI_MODEL`. Re-executing this cell updates kernel state.
- Define class `GeminiEvaluator`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `Field`: Describe a schema field or constrain allowed values.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/rag.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/rag.py)

**Imports:**

- `glob` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_chroma` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_community.document_loaders` — Search, browser, document, and file integrations.
- `langchain_huggingface` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_text_splitters` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `Retriever` | class |
| 125 | `get_retriever` | function |
| 136 | `ingest` | function |
| 148 | `ChromaRAG` | class |
| 18 | `__init__` | function |
| 38 | `_get_documents` | function |
| 52 | `_build_store` | function |
| 78 | `_init_or_load_db` | function |
| 94 | `rebuild` | function |
| 101 | `get_retriever` | function |
| 106 | `get_relevant_docs` | function |
| 116 | `get_relevant_chunks` | function |
| 151 | `__init__` | function |
| 163 | `_ensure_vectorstore` | function |
| 172 | `ingest_documents` | function |
| 196 | `retrieve_context` | function |
| 201 | `get_retrieval_metadata` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `glob`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_text_splitters`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_community.document_loaders`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_huggingface`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_chroma`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DB_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `DIRECTORY_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `CHROMA_PERSIST_DIRECTORY`. Re-executing this cell updates kernel state.
- Store expression result in `TOP_K`. Re-executing this cell updates kernel state.
- Store expression result in `CHUNK_SIZE`. Re-executing this cell updates kernel state.
- Store expression result in `CHUNK_OVERLAP`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL_NAME`. Re-executing this cell updates kernel state.
- Define class `Retriever`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `_GLOBAL_RETRIEVER`. Re-executing this cell updates kernel state.
- Define `get_retriever()`. Its body runs when called.
- Define `ingest(data_dir)`. Its body runs when called.
- Define class `ChromaRAG`. Separate the class definition, constructing an instance, and calling its methods.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `self._retriever.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `retriever.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `docs.extend`: Add every item from an iterable to a list.
- `retr.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/tools.py](../../../../../../1_foundations/community_contributions/ChatBot_with_evaluator_and_notifier/tools.py)

**Imports:**

- `base64` — Local or third-party dependency; follow its definition/import before execution.
- `csv` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `google.oauth2.service_account` — Local or third-party dependency; follow its definition/import before execution.
- `gspread` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 22 | `_get_google_credentials` | function |
| 36 | `_save_to_google_sheets` | function |
| 44 | `_save_to_csv` | function |
| 53 | `_record_user_details` | function |
| 68 | `send_pushover_notification` | function |
| 107 | `collect_user_details` | function |
| 111 | `log_interaction` | function |
| 131 | `notify` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `csv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base64`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `CSV_FILE`. Re-executing this cell updates kernel state.
- Store expression result in `SHEET_NAME`. Re-executing this cell updates kernel state.
- Define `_get_google_credentials()`. Its body runs when called.
- Define `_save_to_google_sheets(email, name, notes)`. Its body runs when called.
- Define `_save_to_csv(email, name, notes)`. Its body runs when called.
- Define `_record_user_details(email, name, notes)`. Its body runs when called.
- Define `send_pushover_notification(message, user_details)`. Its body runs when called.
- Define `collect_user_details(name, email)`. Its body runs when called.
- Define `log_interaction(query, response, evaluation, user_details, csv_path)`. Its body runs when called.
- Define `notify(title, message)`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `client.open`: Open a file or resource. Inspect the mode and path before running it.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `json.dumps`: Convert Python data to JSON text.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
