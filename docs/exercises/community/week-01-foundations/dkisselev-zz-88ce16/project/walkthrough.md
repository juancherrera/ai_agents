# dkisselev zz: project walkthrough

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

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Configuration and supporting files

- [1_foundations/community_contributions/dkisselev-zz/persona_rag/pyproject.toml](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [1_foundations/community_contributions/dkisselev-zz/README.md](../../../../../../1_foundations/community_contributions/dkisselev-zz/README.md): Digital Persona - Personal Knowledge Base. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Module-by-module reading map

### [1_foundations/community_contributions/dkisselev-zz/persona_rag/answer.py](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/answer.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `langchain_chroma` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_core.documents` — Message/tool interfaces.
- `langchain_core.messages` — Message/tool interfaces.
- `langchain_core.output_parsers` — Message/tool interfaces.
- `langchain_core.prompts` — Message/tool interfaces.
- `langchain_huggingface` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_openai` — Compatible model integration for LangChain.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `rank_bm25` — Local or third-party dependency; follow its definition/import before execution.
- `sentence_transformers` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 52 | `get_reranker` | function |
| 63 | `get_bm25` | function |
| 83 | `initialize_retriever` | function |
| 106 | `expand_query` | function |
| 126 | `fetch_context` | function |
| 212 | `format_doc_with_metadata` | function |
| 231 | `answer_question` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_chroma`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_huggingface`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.messages`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.documents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.output_parsers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.prompts`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sentence_transformers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `rank_bm25`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `DATA_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `VECTOR_DB`. Re-executing this cell updates kernel state.
- Store expression result in `EMBEDDING_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `LLM_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `PERSONA_NAME`. Re-executing this cell updates kernel state.
- Store expression result in `RETRIEVAL_K`. Re-executing this cell updates kernel state.
- Store expression result in `FINAL_K`. Re-executing this cell updates kernel state.
- Store expression result in `USE_QUERY_EXPANSION`. Re-executing this cell updates kernel state.
- Store expression result in `USE_HYBRID_SEARCH`. Re-executing this cell updates kernel state.
- Store expression result in `SYSTEM_PROMPT`. Re-executing this cell updates kernel state.
- Store expression result in `embeddings`. Re-executing this cell updates kernel state.
- Store expression result in `vectorstore`. Re-executing this cell updates kernel state.
- Store expression result in `retriever`. Re-executing this cell updates kernel state.
- Store expression result in `llm`. Re-executing this cell updates kernel state.
- Store expression result in `_reranker`. Re-executing this cell updates kernel state.
- Define `get_reranker()`. Its body runs when called.
- Store expression result in `_bm25`. Re-executing this cell updates kernel state.
- Store expression result in `_bm25_docs`. Re-executing this cell updates kernel state.
- Define `get_bm25()`. Its body runs when called.
- Define `initialize_retriever()`. Its body runs when called.
- Store expression result in `output_parser`. Re-executing this cell updates kernel state.
- Store expression result in `template`. Re-executing this cell updates kernel state.
- Store expression result in `query_gen_prompt`. Re-executing this cell updates kernel state.
- Store expression result in `query_gen_chain`. Re-executing this cell updates kernel state.
- Define `expand_query(question)`. Its body runs when called.
- Define `fetch_context(question)`. Its body runs when called.
- Define `format_doc_with_metadata(doc, idx)`. Its body runs when called.
- Define `answer_question(question, history)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `messages.extend`: Add every item from an iterable to a list.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `llm.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `collection.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `query_gen_chain.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `all_queries.extend`: Add every item from an iterable to a list.
- `retriever.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `zip`: Pair items at the same positions; unequal lengths truncate unless checked.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `all_docs.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `doc.metadata.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/dkisselev-zz/persona_rag/data/process_data.py](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/data/process_data.py)

**Imports:**

- `abc` — Local or third-party dependency; follow its definition/import before execution.
- `argparse` — Local or third-party dependency; follow its definition/import before execution.
- `csv` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `sys` — Interpreter/process information.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 53 | `_timestamp_to_date` | function |
| 62 | `parse_linkedin_date` | function |
| 78 | `to_first_person` | function |
| 87 | `safe_load_json` | function |
| 96 | `safe_load_csv` | function |
| 105 | `DataProcessor` | class |
| 130 | `FacebookProcessor` | class |
| 389 | `LinkedInProcessor` | class |
| 629 | `main` | function |
| 108 | `__init__` | function |
| 113 | `log` | function |
| 118 | `add_chunk` | function |
| 126 | `process` | function |
| 133 | `process_profile` | function |
| 187 | `process_posts` | function |
| 207 | `process_comments` | function |
| 232 | `process_messages` | function |
| 256 | `process_list_items` | function |
| 281 | `process_group_membership` | function |
| 307 | `process_saved_items` | function |
| 339 | `process_apps_posts` | function |
| 353 | `process` | function |
| 392 | `process_profile` | function |
| 418 | `process_positions` | function |
| 442 | `process_education` | function |
| 463 | `process_skills` | function |
| 473 | `process_certifications` | function |
| 494 | `process_recommendations_received` | function |
| 517 | `process_publications` | function |
| 538 | `process_projects` | function |
| 558 | `process_comments` | function |
| 576 | `process_volunteering` | function |
| 595 | `process` | function |

**Read in this order:**

- Make available: `argparse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `csv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `abc`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SCRIPT_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `PARENT_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `USER_NAME`. Re-executing this cell updates kernel state.
- Store dictionary in `FACEBOOK_CONFIG`. Re-executing this cell updates kernel state.
- Store dictionary in `LINKEDIN_CONFIG`. Re-executing this cell updates kernel state.
- Define `_timestamp_to_date(timestamp, default)`. Its body runs when called.
- Define `parse_linkedin_date(date_str)`. Its body runs when called.
- Define `to_first_person(text, user_name)`. Its body runs when called.
- Define `safe_load_json(file_path)`. Its body runs when called.
- Define `safe_load_csv(file_path)`. Its body runs when called.
- Define class `DataProcessor` using `ABC`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `FacebookProcessor` using `DataProcessor`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `LinkedInProcessor` using `DataProcessor`. Separate the class definition, constructing an instance, and calling its methods.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `self.chunks.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `profile.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `rel.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `comment_entry.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `group_entry.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `save_entry.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `post.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `row.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `message.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `items.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `attachment.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `bday.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `exp.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/dkisselev-zz/persona_rag/evaluate.py](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/evaluate.py)

**Imports:**

- `answer` — Local or third-party dependency; follow its definition/import before execution.
- `argparse` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `langchain_chroma` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_core.documents` — Message/tool interfaces.
- `langchain_huggingface` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_text_splitters` — Local or third-party dependency; follow its definition/import before execution.
- `math` — Local or third-party dependency; follow its definition/import before execution.
- `matplotlib` — Local or third-party dependency; follow its definition/import before execution.
- `matplotlib.pyplot` — Local or third-party dependency; follow its definition/import before execution.
- `numpy` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pandas` — Tabular data.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `random` — Demo/simulation randomness.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 43 | `TestQuestion` | class |
| 50 | `RetrievalEval` | class |
| 58 | `AnswerEval` | class |
| 66 | `load_json_data` | function |
| 71 | `load_tests` | function |
| 80 | `create_simple_docs` | function |
| 117 | `create_chunks_with_size` | function |
| 128 | `calculate_mrr_simple` | function |
| 136 | `evaluate_chunks` | function |
| 163 | `run_hyperparameter_tuning` | function |
| 263 | `plot_chunk_results` | function |
| 312 | `plot_k_results` | function |
| 347 | `print_tuning_summary` | function |
| 369 | `calculate_mrr` | function |
| 377 | `calculate_dcg` | function |
| 384 | `calculate_ndcg` | function |
| 403 | `evaluate_retrieval` | function |
| 429 | `evaluate_answer` | function |
| 478 | `run_evaluation` | function |
| 573 | `print_retrieval_results` | function |
| 596 | `print_answer_results` | function |
| 615 | `save_evaluation_results` | function |
| 659 | `compare_rag_configurations` | function |
| 748 | `parse_list_arg` | function |
| 752 | `main` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `random`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `argparse`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `math`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pandas`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `numpy`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `matplotlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `matplotlib.pyplot`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.documents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_text_splitters`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_chroma`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_huggingface`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `answer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SCRIPT_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `DATA_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `FACEBOOK_DATA`. Re-executing this cell updates kernel state.
- Store expression result in `LINKEDIN_DATA`. Re-executing this cell updates kernel state.
- Store expression result in `TESTS_FILE`. Re-executing this cell updates kernel state.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Define class `TestQuestion` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RetrievalEval` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `AnswerEval` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define `load_json_data(filepath)`. Its body runs when called.
- Define `load_tests()`. Its body runs when called.
- Define `create_simple_docs(facebook_items, linkedin_items)`. Its body runs when called.
- Define `create_chunks_with_size(documents, chunk_size, chunk_overlap_ratio)`. Its body runs when called.
- Define `calculate_mrr_simple(keyword, retrieved_docs)`. Its body runs when called.
- Define `evaluate_chunks(chunks, tests, chunk_size, embeddings_model, k)`. Its body runs when called.
- Define `run_hyperparameter_tuning(chunk_sizes, k_values, sample_size)`. Its body runs when called.
- Define `plot_chunk_results(chunk_df, best_chunk)`. Its body runs when called.
- Define `plot_k_results(k_df, best_k)`. Its body runs when called.
- Define `print_tuning_summary(chunk_df, k_df, best_chunk, best_k)`. Its body runs when called.
- Define `calculate_mrr(keyword, retrieved_docs)`. Its body runs when called.
- Define `calculate_dcg(relevances, k)`. Its body runs when called.
- Define `calculate_ndcg(keyword, retrieved_docs, k)`. Its body runs when called.
- Define `evaluate_retrieval(test, k)`. Its body runs when called.
- Define `evaluate_answer(test)`. Its body runs when called.
- Define `run_evaluation(answer_sample_size, config_name)`. Its body runs when called.
- Define `print_retrieval_results(retrieval_df)`. Its body runs when called.
- Define `print_answer_results(answer_df)`. Its body runs when called.
- Define `save_evaluation_results(retrieval_df, retrieval_results, answer_results)`. Its body runs when called.
- Define `compare_rag_configurations(answer_sample_size)`. Its body runs when called.
- Define `parse_list_arg(arg_str)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `Field`: Describe a schema field or constrain allowed values.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `plt.close`: Release the resource; match setup with cleanup.
- `client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `best_accuracy.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `docs.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `chunk_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `k_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `all_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `tests.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `retriever.invoke`: Run a synchronous graph, tool, or model operation. Inspect the receiver.
- `mrr_scores.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `retrieval_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `answer_results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `x.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `doc.metadata.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `r.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/dkisselev-zz/persona_rag/ingest.py](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/ingest.py)

**Imports:**

- `collections` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `json` — Encoding/decoding structured text.
- `langchain_chroma` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_core.documents` — Message/tool interfaces.
- `langchain_huggingface` — Local or third-party dependency; follow its definition/import before execution.
- `langchain_text_splitters` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 28 | `load_json_data` | function |
| 33 | `group_linkedin_data` | function |
| 110 | `group_facebook_data` | function |
| 246 | `create_chunks` | function |
| 257 | `create_embeddings` | function |
| 285 | `main` | function |

**Read in this order:**

- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `collections`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_core.documents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_chroma`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_text_splitters`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `langchain_huggingface`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `SCRIPT_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `DATA_DIR`. Re-executing this cell updates kernel state.
- Store expression result in `FACEBOOK_DATA`. Re-executing this cell updates kernel state.
- Store expression result in `LINKEDIN_DATA`. Re-executing this cell updates kernel state.
- Store expression result in `VECTOR_DB`. Re-executing this cell updates kernel state.
- Store expression result in `EMBEDDING_MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `CHUNK_SIZE`. Re-executing this cell updates kernel state.
- Store expression result in `CHUNK_OVERLAP`. Re-executing this cell updates kernel state.
- Define `load_json_data(filepath)`. Its body runs when called.
- Define `group_linkedin_data(items)`. Its body runs when called.
- Define `group_facebook_data(items)`. Its body runs when called.
- Define `create_chunks(documents)`. Its body runs when called.
- Define `create_embeddings(chunks)`. Its body runs when called.
- Define `main()`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `grouped_docs.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `collection.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `text_parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `doc.metadata.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `no_timestamp.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/dkisselev-zz/persona_rag/persona_app.py](../../../../../../1_foundations/community_contributions/dkisselev-zz/persona_rag/persona_app.py)

**Imports:**

- `answer` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `json` — Encoding/decoding structured text.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `requests` — Synchronous HTTP requests.

| Source line | Definition | Kind |
| --- | --- | --- |
| 30 | `push` | function |
| 45 | `record_user_details` | function |
| 50 | `record_unknown_question` | function |
| 101 | `handle_tool_calls` | function |
| 151 | `chat` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `answer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `openai_client`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Store expression result in `MODEL`. Re-executing this cell updates kernel state.
- Store expression result in `PERSONA_NAME`. Re-executing this cell updates kernel state.
- Define `push(message)`. Its body runs when called.
- Define `record_user_details(email, name, notes)`. Its body runs when called.
- Define `record_unknown_question(question)`. Its body runs when called.
- Store dictionary in `record_user_details_json`. Re-executing this cell updates kernel state.
- Store dictionary in `record_unknown_question_json`. Re-executing this cell updates kernel state.
- Store list in `tools`. Re-executing this cell updates kernel state.
- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- Store expression result in `SYSTEM_PROMPT`. Re-executing this cell updates kernel state.
- Store expression result in `SYSTEM_PROMPT_POST_CONTACT`. Re-executing this cell updates kernel state.
- Define `chat(message, history)`. Its body runs when called.
- Store expression result in `demo`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `demo.launch`: Start the UI/server; defining the UI alone does not start it.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `openai_client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `json.dumps`: Convert Python data to JSON text.
- `h.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `messages.extend`: Add every item from an iterable to a list.
- `doc.metadata.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tc.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tc.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
