# salah: project walkthrough

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

### Configure a real SDK agent and inspect its tool schema

Agent holds instructions/model/tools. The function_tool decorator constructs a schema from the signature and docstring. Constructing this configuration does not run the agent or contact a provider.

**Ask yourself:** Which argument names will appear in the registered tool schema?

**Practice:** Change the docstring and inspect the tool description. Find Runner.run in the source: that is where the interaction loop actually begins.

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

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

## Module-by-module reading map

### [2_openai/community_contributions/salah/security-vuln-advisor/app.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/app.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `asyncio` — Coroutines, scheduling, and concurrency.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `dotenv` — Load explicit environment-file configuration.
- `gradio` — Python callback-based web/chat interfaces.
- `os` — Process configuration and operating-system paths.
- `scanner_agents` — Local or third-party dependency; follow its definition/import before execution.
- `services` — Local or third-party dependency; follow its definition/import before execution.
- `utils` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 31 | `_format_final_report` | function |
| 37 | `run_scan` | async function |
| 120 | `handle_clear` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `gradio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `services`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `scanner_agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Store expression result in `scanner`. Re-executing this cell updates kernel state.
- Store expression result in `analyzer`. Re-executing this cell updates kernel state.
- Store expression result in `advisor`. Re-executing this cell updates kernel state.
- Store expression result in `reporter`. Re-executing this cell updates kernel state.
- Store expression result in `email_agent`. Re-executing this cell updates kernel state.
- Define `_format_final_report(report, trace_url)`. Its body runs when called.
- Define `run_scan(image_name, min_severity)`. Its body runs when called; await/schedule the coroutine to execute async work.
- Keep a resource open for the block, then clean it up. Server/tool calls must remain inside the connection lifetime.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `ui.launch`: Start the UI/server; defining the UI alone does not start it.
- `trace`: Group activity for inspection. Configured tracing may export run data.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/config.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/config.py)

**Imports:**

- `dotenv` — Load explicit environment-file configuration.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `Config` | class |
| 27 | `validate` | function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dotenv`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Config`. Separate the class definition, constructing an instance, and calling its methods.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/models.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/models.py)

**Imports:**

- `enum` — Local or third-party dependency; follow its definition/import before execution.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `SeverityLevel` | class |
| 14 | `ExploitabilityLevel` | class |
| 21 | `ConfidenceLevel` | class |
| 27 | `EffortLevel` | class |
| 34 | `RiskLevel` | class |
| 42 | `Vulnerability` | class |
| 52 | `AnalyzedVulnerability` | class |
| 65 | `VulnerabilityAnalysis` | class |
| 76 | `RemediationStep` | class |
| 92 | `RemediationPlan` | class |
| 108 | `RemediationPlans` | class |
| 117 | `SecurityReport` | class |
| 142 | `ScanResult` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `enum`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SeverityLevel` using `str`, `Enum`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ExploitabilityLevel` using `str`, `Enum`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ConfidenceLevel` using `str`, `Enum`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `EffortLevel` using `str`, `Enum`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RiskLevel` using `str`, `Enum`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Vulnerability` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `AnalyzedVulnerability` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `VulnerabilityAnalysis` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RemediationStep` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RemediationPlan` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `RemediationPlans` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `SecurityReport` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ScanResult` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/__init__.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/__init__.py)

**Imports:**

- `.email_agent` — Local or third-party dependency; follow its definition/import before execution.
- `.remediation_advisor` — Local or third-party dependency; follow its definition/import before execution.
- `.report_generator` — Local or third-party dependency; follow its definition/import before execution.
- `.vulnerability_analyzer` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.vulnerability_analyzer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.remediation_advisor`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.report_generator`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `.email_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/email_agent.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/email_agent.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `logging` — Application diagnostics.
- `os` — Process configuration and operating-system paths.
- `sendgrid` — External email delivery.
- `sendgrid.helpers.mail` — External email delivery.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 13 | `send_email` | function |
| 25 | `EmailAgent` | class |
| 33 | `__init__` | function |
| 38 | `_create_agent` | function |
| 47 | `send_report` | async function |

**Read in this order:**

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `sendgrid.helpers.mail`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define `send_email(subject, html_body)`. Its body runs when called.
- Define class `EmailAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Mail(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.client.mail.send.post`: Send an HTTP POST. Notification/email calls have external effects.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/remediation_advisor.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/remediation_advisor.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `RemediationAdvisorAgent` | class |
| 44 | `__init__` | function |
| 49 | `_create_agent` | function |
| 59 | `advise` | async function |
| 97 | `_create_empty_plans` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `RemediationAdvisorAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `json.dumps`: Convert Python data to JSON text.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/report_generator.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/report_generator.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `models` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 17 | `ReportGeneratorAgent` | class |
| 50 | `__init__` | function |
| 55 | `_create_agent` | function |
| 64 | `generate_report` | async function |
| 109 | `format_report_markdown` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `ReportGeneratorAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/vulnerability_analyzer.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/scanner_agents/vulnerability_analyzer.py)

**Imports:**

- `agents` — OpenAI Agents SDK orchestration, tools, guardrails, tracing.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 18 | `VulnerabilityAnalyzerAgent` | class |
| 43 | `__init__` | function |
| 48 | `_create_agent` | function |
| 57 | `analyze` | async function |
| 92 | `_create_empty_analysis` | function |

**Read in this order:**

- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `agents`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `VulnerabilityAnalyzerAgent`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `json.dumps`: Convert Python data to JSON text.
- `Runner.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/services/__init__.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/services/__init__.py)

**Imports:**

- `.image_scanner` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.image_scanner`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [2_openai/community_contributions/salah/security-vuln-advisor/services/image_scanner.py](../../../../../../2_openai/community_contributions/salah/security-vuln-advisor/services/image_scanner.py)

**Imports:**

- `asyncio` — Coroutines, scheduling, and concurrency.
- `config` — Local or third-party dependency; follow its definition/import before execution.
- `json` — Encoding/decoding structured text.
- `logging` — Application diagnostics.
- `models` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `subprocess` — Launch/manage another process.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 14 | `ImageScannerService` | class |
| 28 | `__init__` | function |
| 32 | `_validate_scanner` | function |
| 51 | `scan_image` | async function |
| 77 | `_run_trivy_scan_async` | async function |
| 128 | `_parse_trivy_output` | function |
| 170 | `_extract_cvss_score` | function |
| 184 | `_filter_by_severity` | function |

**Read in this order:**

- Make available: `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `asyncio`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `logging`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `config`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `logger`. Re-executing this cell updates kernel state.
- Define class `ImageScannerService`. Separate the class definition, constructing an instance, and calling its methods.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.SEVERITY_ORDER.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `result.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `vuln.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `vulnerabilities.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
