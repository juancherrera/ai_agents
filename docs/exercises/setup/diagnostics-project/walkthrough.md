# setup: project walkthrough

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

### A callback receives a message and history

A UI invokes your function and displays its return value. Earlier turns are an input, not automatic model memory. This practice calls the callback directly without starting a server.

**Ask yourself:** What changes when history contains two old messages?

**Practice:** Make the callback return an uppercase version of the question. Compare returning a value with printing it.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

### Request, status, and parsed body

An HTTP client sends a request and returns a status plus response body. Provider requests are network side effects; a fixture lets you practice handling success/failure without sending one.

**Ask yourself:** Will a non-200 response be treated as success here?

**Practice:** Add a 429 fixture and distinguish rate limiting from authentication. Explain why real calls need timeouts.

## Module-by-module reading map

### [setup/diagnostics.py](../../../../setup/diagnostics.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `pkg_resources` — Local or third-party dependency; follow its definition/import before execution.
- `platform` — Local or third-party dependency; follow its definition/import before execution.
- `psutil` — Local or third-party dependency; follow its definition/import before execution.
- `requests` — Synchronous HTTP requests.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.
- `site` — Local or third-party dependency; follow its definition/import before execution.
- `speedtest` — Local or third-party dependency; follow its definition/import before execution.
- `ssl` — Local or third-party dependency; follow its definition/import before execution.
- `subprocess` — Launch/manage another process.
- `sys` — Interpreter/process information.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `time` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `Diagnostics` | class |
| 16 | `__init__` | function |
| 22 | `log` | function |
| 27 | `start` | function |
| 31 | `end` | function |
| 38 | `_log_error` | function |
| 42 | `_log_warning` | function |
| 46 | `run` | function |
| 74 | `_step1_system_info` | function |
| 117 | `_step2_check_files` | function |
| 143 | `_step3_git_repo` | function |
| 172 | `_step4_check_env_file` | function |
| 206 | `_step6_virtualenv_check` | function |
| 227 | `_check_python_packages` | function |
| 259 | `_step7_network_connectivity` | function |
| 325 | `_step8_environment_variables` | function |
| 354 | `_step9_additional_diagnostics` | function |
| 364 | `is_in_site_packages` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `platform`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ssl`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Diagnostics`. Separate the class definition, constructing an instance, and calling its methods.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `diagnostics.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `self.errors.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.warnings.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.start`: Start the receiver runtime/server/resource.
- `os.remove`: Delete an item/file depending on the receiver. Check the target.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `test_file.unlink`: Delete a filesystem path. Do not run it just to inspect a source example.
- `site_packages_paths.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `requests.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
