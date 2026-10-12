# 4 lab4 with rag

Source: [1_foundations/community_contributions/jongkook/4_lab4_with_rag.ipynb](../../../../../../../1_foundations/community_contributions/jongkook/4_lab4_with_rag.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → A tool schema describes; a dispatcher executes → An agent loop must make progress and stop → Structured output validates shape → Evaluate, give feedback, and bound revision → A callback receives a message and history → Extracted documents become prompt context → Retrieve evidence before answering

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

## Follow the source in order

### Cell 1: code

- Make available: `sentence_transformers`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- Define `push(message)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Define `record_user_details(email, name, notes)`. Its body runs when called.
- Store dictionary in `record_user_details_json`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Define `record_unknown_question(question)`. Its body runs when called.
- Store dictionary in `record_unknown_question_json`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Store list in `tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `chromadb`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `collection_name`. Re-executing this cell updates kernel state.
- Store expression result in `chroma_client`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `collection`. Re-executing this cell updates kernel state.
- Store expression result in `resume_txt`. Re-executing this cell updates kernel state.
- Store expression result in `resume_reader`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Define `chunk_text(text, chunk_size, overlap)`. Its body runs when called.
- Store expression result in `resume_chunks`. Re-executing this cell updates kernel state.
- Store expression result in `embedding_model`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Store expression result in `linkedin`. Re-executing this cell updates kernel state.
- Store expression result in `linkedin_profile`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `chunks.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Store expression result in `name`. Re-executing this cell updates kernel state.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Evaluation` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store formatted string in `avator_system_prompt`. Re-executing this cell updates kernel state.
- Define `avator(message, history, evaluation)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI(...).chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store formatted string in `evaluator_system_prompt`. Re-executing this cell updates kernel state.
- Define `evaluator_user_prompt(question, avator_response, history)`. Its body runs when called.
- Define `evaluator(question, avator_response, history)`. Its body runs when called.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `llm_client.beta.chat.completions.parse`: Parse syntax/data or request structured model output, depending on the receiver. Inspect response_format when this is a completions.parse call.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store expression result in `max_attempt`. Re-executing this cell updates kernel state.
- Define `orchestrator(message, history)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `gradio.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `gradio.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 2 | `push` | function |
| 3 | `record_user_details` | function |
| 4 | `record_unknown_question` | function |
| 6 | `handle_tool_calls` | function |
| 7 | `chunk_text` | function |
| 8 | `Evaluation` | class |
| 9 | `avator` | function |
| 10 | `evaluator_user_prompt` | function |
| 10 | `evaluator` | function |
| 11 | `orchestrator` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
