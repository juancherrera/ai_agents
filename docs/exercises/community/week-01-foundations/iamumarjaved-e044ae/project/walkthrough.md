# iamumarjaved: project walkthrough

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

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/iamumarjaved/README.md](../../../../../../1_foundations/community_contributions/iamumarjaved/README.md): Advanced Digital Twin with RAG. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/iamumarjaved/app.py](../../../../../../1_foundations/community_contributions/iamumarjaved/app.py)

**Imports:**

- `evaluation` — Local or third-party dependency; follow its definition/import before execution.
- `gradio` — Python callback-based web/chat interfaces.
- `helpers` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `pathlib` — Explicit filesystem paths.
- `rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `DigitalTwin` | class |
| 245 | `chat_wrapper` | function |
| 17 | `__init__` | function |
| 101 | `record_user_details` | function |
| 108 | `record_unknown_question` | function |
| 112 | `search_knowledge_base` | function |
| 129 | `handle_tool_calls` | function |
| 146 | `get_system_prompt` | function |
| 181 | `chat` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `helpers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rag_system`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `evaluation`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `DigitalTwin`. Separate the class definition, constructing an instance, and calling its methods.
- Store expression result in `twin`. Re-executing this cell updates kernel state.
- Define `chat_wrapper(message, history)`. Its body runs when called.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `json.dumps`: Convert Python data to JSON text.
- `converted_history.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/evaluation.py](../../../../../../1_foundations/community_contributions/iamumarjaved/evaluation.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `helpers` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `pandas` — Tabular data.
- `pathlib` — Explicit filesystem paths.
- `rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `RAGEvaluator` | class |
| 198 | `create_test_cases` | function |
| 11 | `__init__` | function |
| 14 | `mean_reciprocal_rank` | function |
| 20 | `dcg_at_k` | function |
| 27 | `ndcg_at_k` | function |
| 36 | `precision_at_k` | function |
| 41 | `recall_at_k` | function |
| 48 | `llm_as_judge_relevance` | function |
| 74 | `llm_as_judge_answer` | function |
| 110 | `evaluate_retrieval` | function |
| 136 | `evaluate_rag_system` | function |
| 155 | `compare_rag_methods` | function |
| 168 | `save_evaluation_report` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `RAGEvaluator`. Separate the class definition, constructing an instance, and calling its methods.
- Define `create_test_cases(queries_and_answers)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `relevance_scores.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `test_case.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `all_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/helpers/__init__.py](../../../../../../1_foundations/community_contributions/iamumarjaved/helpers/__init__.py)

**Imports:**

- `.config` — Local or third-party dependency; follow its definition/import before execution.
- `.data_loader` — Local or third-party dependency; follow its definition/import before execution.
- `.notification` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.data_loader`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.notification`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/helpers/config.py](../../../../../../1_foundations/community_contributions/iamumarjaved/helpers/config.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `get_config` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_config()`. Its body runs when called.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/helpers/data_loader.py](../../../../../../1_foundations/community_contributions/iamumarjaved/helpers/data_loader.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.
- `pypdf` — Embedded PDF-text extraction.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `load_pdf` | function |
| 16 | `load_text_file` | function |
| 21 | `load_all_documents` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `load_pdf(file_path)`. Its body runs when called.
- Define `load_text_file(file_path)`. Its body runs when called.
- Define `load_all_documents(base_path)`. Its body runs when called.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/helpers/notification.py](../../../../../../1_foundations/community_contributions/iamumarjaved/helpers/notification.py)

**Imports:**

- `requests` — Synchronous HTTP requests.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `PushoverNotifier` | class |
| 7 | `__init__` | function |
| 13 | `send` | function |

**Read in this order:**

- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `PushoverNotifier`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/rag_system.py](../../../../../../1_foundations/community_contributions/iamumarjaved/rag_system.py)

**Imports:**

- `chromadb` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `pickle` — Local or third-party dependency; follow its definition/import before execution.
- `rank_bm25` — Local or third-party dependency; follow its definition/import before execution.
- `sentence_transformers` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `QueryExpander` | class |
| 38 | `HybridRetriever` | class |
| 216 | `RAGSystem` | class |
| 14 | `__init__` | function |
| 17 | `expand_query` | function |
| 40 | `__init__` | function |
| 55 | `chunk_text` | function |
| 64 | `index_documents` | function |
| 109 | `retrieve_bm25` | function |
| 123 | `retrieve_semantic` | function |
| 138 | `retrieve_hybrid` | function |
| 170 | `rerank` | function |
| 180 | `retrieve` | function |
| 218 | `__init__` | function |
| 224 | `load_knowledge_base` | function |
| 227 | `generate_answer` | function |
| 244 | `query` | function |
| 142 | `normalize_scores` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pickle`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sentence_transformers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rank_bm25`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `chromadb`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `QueryExpander`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `HybridRetriever`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RAGSystem`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `bm25_scores.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `semantic_scores.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `chunks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `all_chunks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `retrieved.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `kwargs.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/test_rag.py](../../../../../../1_foundations/community_contributions/iamumarjaved/test_rag.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `evaluation` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rag_system`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `evaluation`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai_client`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `test_query`. Re-executing this cell updates kernel state.
- Store list in `methods_to_test`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/tests/__init__.py](../../../../../../1_foundations/community_contributions/iamumarjaved/tests/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/tests/run_all_tests.py](../../../../../../1_foundations/community_contributions/iamumarjaved/tests/run_all_tests.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.
- `sys` — Interpreter/process information.
- `tests.test_evaluation` — Local or third-party dependency; follow its definition/import before execution.
- `tests.test_helpers` — Local or third-party dependency; follow its definition/import before execution.
- `tests.test_rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `main` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tests.test_helpers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tests.test_rag_system`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tests.test_evaluation`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/tests/test_evaluation.py](../../../../../../1_foundations/community_contributions/iamumarjaved/tests/test_evaluation.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `evaluation` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `test_mrr_calculation` | function |
| 39 | `test_ndcg_calculation` | function |
| 62 | `test_precision_recall` | function |
| 92 | `test_llm_as_judge` | function |
| 126 | `test_create_test_cases` | function |
| 152 | `test_rag_evaluation` | function |
| 187 | `run_all_tests` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `evaluation`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rag_system`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_mrr_calculation()`. Its body runs when called.
- Define `test_ndcg_calculation()`. Its body runs when called.
- Define `test_precision_recall()`. Its body runs when called.
- Define `test_llm_as_judge()`. Its body runs when called.
- Define `test_create_test_cases()`. Its body runs when called.
- Define `test_rag_evaluation()`. Its body runs when called.
- Define `run_all_tests()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/tests/test_helpers.py](../../../../../../1_foundations/community_contributions/iamumarjaved/tests/test_helpers.py)

**Imports:**

- `helpers.config` — Local or third-party dependency; follow its definition/import before execution.
- `helpers.data_loader` — Local or third-party dependency; follow its definition/import before execution.
- `helpers.notification` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `test_data_loader` | function |
| 32 | `test_pushover_notifier` | function |
| 56 | `test_config` | function |
| 83 | `run_all_tests` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `helpers.data_loader`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `helpers.notification`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `helpers.config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_data_loader()`. Its body runs when called.
- Define `test_pushover_notifier()`. Its body runs when called.
- Define `test_config()`. Its body runs when called.
- Define `run_all_tests()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/iamumarjaved/tests/test_rag_system.py](../../../../../../1_foundations/community_contributions/iamumarjaved/tests/test_rag_system.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `rag_system` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `test_query_expansion` | function |
| 41 | `test_retriever_initialization` | function |
| 63 | `test_chunking` | function |
| 87 | `test_document_indexing` | function |
| 119 | `test_retrieval_methods` | function |
| 160 | `test_rag_system` | function |
| 200 | `run_all_tests` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rag_system`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_query_expansion()`. Its body runs when called.
- Define `test_retriever_initialization()`. Its body runs when called.
- Define `test_chunking()`. Its body runs when called.
- Define `test_document_indexing()`. Its body runs when called.
- Define `test_retrieval_methods()`. Its body runs when called.
- Define `test_rag_system()`. Its body runs when called.
- Define `run_all_tests()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
