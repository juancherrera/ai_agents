# Profile assistant with tool calls

Source: [1_foundations/4_lab4.ipynb](../../../../../1_foundations/4_lab4.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Describe recording functions → construct schemas → decode requested arguments → execute an allowed function → append matching tool results → ask the model again.

**Before source execution:** The source push calls deliver notifications and globals() dispatch can expose more than intended. Practice uses a specific registry and in-memory recording; the source loop is unbounded.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### A tool schema describes; a dispatcher executes

The model can request a function call. Your application decodes arguments, recognizes the name, executes the function, and returns a matching tool result. No model is needed to practice this boundary.

**Ask yourself:** Does defining the schema call the function?

**Practice:** Change the requested name to an unknown function and observe the error. Then add a harmless second tool to the registry.

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Drafting and delivering are separate steps

An agent that formats text is different from a tool that sends email or notifications. This exercise records a delivery intention in memory only.

**Ask yourself:** Will constructing this draft contact anyone?

**Practice:** Format the body as HTML but keep dry_run_send unchanged. Explain where real delivery would begin.

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

## Follow the source in order

### Cell 1: The first big project - Professionally You!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: code

- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `requests`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pypdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Store expression result in `openai`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Store expression result in `pushover_user`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_token`. Re-executing this cell updates kernel state.
- Store expression result in `pushover_url`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Define `push(message)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `requests.post`: Send an HTTP POST. Notification/email calls have external effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Define `record_user_details(email, name, notes)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Define `record_unknown_question(question)`. Its body runs when called.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Store dictionary in `record_user_details_json`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Store dictionary in `record_unknown_question_json`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Store list in `tools`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Inspect the evaluated value or expression; displaying it does not necessarily modify state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Define `handle_tool_calls(tool_calls)`. Its body runs when called.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `results.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tool`: Register a function as a tool using a decorator or wrapper.
- `json.dumps`: Convert Python data to JSON text.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Store expression result in `reader`. Re-executing this cell updates kernel state.
- Store expression result in `linkedin`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Store expression result in `name`. Re-executing this cell updates kernel state.
- `PdfReader`: Open a PDF reader; page extraction is a separate operation.
- `page.extract_text`: Read embedded page text. A scanned image may produce no useful text.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Store formatted string in `system_prompt`. Re-executing this cell updates kernel state.
- Append/update formatted string in `system_prompt`. Re-executing this cell updates kernel state.
- Append/update formatted string in `system_prompt`. Re-executing this cell updates kernel state.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Define `chat(message, history)`. Its body runs when called.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `messages.extend`: Add every item from an iterable to a list.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: code

- `gr.ChatInterface(...).launch`: Start the UI/server; defining the UI alone does not start it.
- `gr.ChatInterface`: Wire a callback to a chat UI; history is supplied to that callback.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 20: And now for deployment

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: Exercise

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 22: Commercial implications

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 5 | `push` | function |
| 7 | `record_user_details` | function |
| 8 | `record_unknown_question` | function |
| 13 | `handle_tool_calls` | function |
| 15 | `handle_tool_calls` | function |
| 18 | `chat` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../configuration.md)
