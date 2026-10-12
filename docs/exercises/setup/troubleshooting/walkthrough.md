# Diagnose interpreter, configuration, and API failures

Source: [setup/troubleshooting.ipynb](../../../../setup/troubleshooting.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Check interpreter → check installed imports → verify explicit configuration path → check variable presence → inspect the provider error → investigate network separately.

**Before source execution:** The source includes cells that print full keys or put a key in code. Practice deliberately does neither. Diagnostics may use the network and write reports; it is not run automatically.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Working directory vs. source directory

Relative paths start at the process working directory. They do not automatically start next to a notebook. This example inspects paths only and never creates or deletes files.

**Ask yourself:** What folder is the starting point for Path(".")?

**Practice:** Compare the path from the repository root with the path from a week folder. Explain why cd changes relative-file behavior.

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

### Reproduce, inspect, simplify, fix

A small failing case separates causes. Catch the exact exception in a teaching example so the notebook can continue, then implement a specific repair.

**Ask yourself:** Will the original operation raise NameError or KeyError?

**Practice:** Remove the record definition in a scratch cell. Explain how the new NameError differs from the original missing-key error.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Follow the source in order

### Cell 1: Oh dear!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Before we begin

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `urllib.request`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: Another mention of occasional "gotchas" for PC people

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: Step 1

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `venv_name`. Re-executing this cell updates kernel state.
- Store expression result in `virtual_env`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: Step 1.1

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 8: code

- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: Step 2

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 11: code

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `parent_dir`. Re-executing this cell updates kernel state.
- Store expression result in `env_path`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `env_path.open`: Open a file or resource. Inspect the mode and path before running it.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: Fallback plan - python code to create the .env file for you

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 13: code

- Store expression result in `make_me_a_file_with_this_key`. Re-executing this cell updates kernel state.
- Store expression result in `overwrite_if_already_exists`. Re-executing this cell updates kernel state.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `parent_dir`. Re-executing this cell updates kernel state.
- Store expression result in `env_path`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `env_path.open`: Open a file or resource. Inspect the mode and path before running it.
- `env_file.write`: Write data to a file-like object; inspect the path and mode.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: Step 3

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 15: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `api_key`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: It should print some checks including something like:

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 17: Step 4

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 18: code

- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `my_api_key`. Re-executing this cell updates kernel state.
- Store expression result in `openai`. Re-executing this cell updates kernel state.
- Store expression result in `completion`. Re-executing this cell updates kernel state.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `openai.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 19: If the key was set correctly, and this still didn't work

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 20: Gathering Essential Diagnostic information

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 21: code

- Make available: `diagnostics`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- `Diagnostics(...).run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
