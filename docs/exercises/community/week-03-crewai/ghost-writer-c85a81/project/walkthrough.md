# ghost writer: project walkthrough

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Streaming sends partial progress

A generator yields multiple updates. A streamed API can include multiple event types, so a UI should select the text events rather than assume every event is text.

**Ask yourself:** How many text chunks are selected?

**Practice:** Insert a non-text event between the chunks. Confirm it does not become displayed answer text.

### Structured output validates shape

Pydantic models name the fields you expect and check constraints. This helps callers use results reliably, but a valid field can still contain a false claim.

**Ask yourself:** Will confidence=2 pass the constraint?

**Practice:** Add a field named follow_up_questions with type list[str]. Construct a valid object using it.

### Roles, tasks, and process

CrewAI separates who does work from what work is required. A sequential process has a fixed task order; a hierarchical process gives a manager coordination responsibility.

**Ask yourself:** Is a role description itself an executable task?

**Practice:** Add a reviewer role and task. Describe whether its input should include the analyst output.

### Configuration fills named placeholders

The crew entry point supplies inputs, and YAML descriptions contain placeholders. Configuration names must match the inputs used at runtime.

**Ask yourself:** What happens if motion is not supplied?

**Practice:** Add a second placeholder named audience and provide its value. Identify matching input keys in the linked source main.py.

### Text and image inputs are different content items

A multimodal message can contain text plus an image. A structured description can constrain fields such as orientation. This local example constructs a tiny image and validates a description without a model.

**Ask yourself:** Which orientation is valid for an image wider than it is tall?

**Practice:** Create a square image and update orientation. Explain that valid orientation syntax alone does not establish what a model saw.

## Configuration and supporting files

- [3_crew/community_contributions/ghost_writer/pyproject.toml](../../../../../../3_crew/community_contributions/ghost_writer/pyproject.toml): project dependencies and command entry points. Use this project environment when it differs from the root.
- [3_crew/community_contributions/ghost_writer/README.md](../../../../../../3_crew/community_contributions/ghost_writer/README.md): Ghost Writer. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.
- [3_crew/community_contributions/ghost_writer/src/ghost_writer/config/agents.yaml](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/config/agents.yaml): configuration sections `idea_developer`, `plot_developer`, `character_developer`, `art_director`, `outline_developer`, `author`. Match these names with code references and supplied input placeholders.
- [3_crew/community_contributions/ghost_writer/src/ghost_writer/config/tasks.yaml](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/config/tasks.yaml): configuration sections `ideation_task`, `plot_development_task`, `character_development_task`, `book_development_task`, `artistic_vision_task`, `act1_development_task`, `act2_development_task`, `act3_development_task`. Match these names with code references and supplied input placeholders.

## Module-by-module reading map

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/__init__.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/crew.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/crew.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `crewai.agents.agent_builder.base_agent` — Roles, tasks, crews, and workflow configuration.
- `crewai.project` — Roles, tasks, crews, and workflow configuration.
- `ghost_writer.models` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.book_writer_service` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.illustrator_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.transcribe_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.utils.filesystem_utils` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.utils.markdown_utils` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 15 | `GhostWriter` | class |
| 27 | `on_before_kickoff` | function |
| 38 | `idea_developer` | function |
| 45 | `plot_developer` | function |
| 52 | `character_developer` | function |
| 59 | `art_director` | function |
| 66 | `outline_developer` | function |
| 73 | `author` | function |
| 80 | `ideation_task` | function |
| 87 | `plot_development_task` | function |
| 94 | `character_development_task` | function |
| 100 | `on_act_created` | function |
| 105 | `on_book_created` | function |
| 111 | `book_development_task` | function |
| 118 | `on_artistic_vision_created` | function |
| 123 | `artistic_vision_task` | function |
| 131 | `act1_development_task` | function |
| 139 | `act2_development_task` | function |
| 147 | `act3_development_task` | function |
| 155 | `crew` | function |

**Read in this order:**

- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.project`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.agents.agent_builder.base_agent`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.book_writer_service`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.transcribe_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.illustrator_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.filesystem_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.markdown_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `GhostWriter`. Separate the class definition, constructing an instance, and calling its methods.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `Task`: Describe assigned work and expected output.
- `Crew`: Combine roles, tasks, and an execution process.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/main.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/main.py)

**Imports:**

- `datetime` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.crew` — Local or third-party dependency; follow its definition/import before execution.
- `sys` — Interpreter/process information.
- `warnings` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 16 | `run` | function |

**Read in this order:**

- Make available: `sys`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `warnings`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.crew`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `run()`. Its body runs when called.
- `GhostWriter(...).crew(...).kickoff`: Execute a CrewAI crew using the supplied inputs; it can call models, tools, and write outputs.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/models.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/models.py)

**Imports:**

- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `Idea` | class |
| 11 | `Plot` | class |
| 19 | `Character` | class |
| 29 | `Characters` | class |
| 33 | `Scene` | class |
| 40 | `Chapter` | class |
| 47 | `Act` | class |
| 55 | `Book` | class |
| 60 | `ArtisticVision` | class |

**Read in this order:**

- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `Idea` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Plot` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Character` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Characters` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Scene` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Chapter` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Act` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `Book` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ArtisticVision` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/__init__.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/__init__.py)

**Imports:**

- `.book_writer_service` — Local or third-party dependency; follow its definition/import before execution.

**Read in this order:**

- Make available: `.book_writer_service`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `__all__`. Re-executing this cell updates kernel state.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/book_writer_service.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/book_writer_service.py)

**Imports:**

- `ghost_writer.models` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.illustration_writer` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.scene_writer` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.writer_templates` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.convert_to_pdf_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.illustrator_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.transcribe_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.utils.markdown_utils` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 12 | `NullIllustrator` | class |
| 17 | `BookWriterService` | class |
| 13 | `run` | function |
| 18 | `__init__` | function |
| 53 | `set_artistic_vision` | function |
| 56 | `write_chapter` | function |
| 78 | `write_act` | function |
| 85 | `write_book_cover` | function |
| 98 | `save_pdf` | function |

**Read in this order:**

- Make available: `ghost_writer.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.convert_to_pdf_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.transcribe_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.illustrator_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.markdown_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.illustration_writer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.scene_writer`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.writer_templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `NullIllustrator`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `BookWriterService`. Separate the class definition, constructing an instance, and calling its methods.
- `self.transcriber.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.pdf_tool.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/illustration_writer.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/illustration_writer.py)

**Imports:**

- `ghost_writer.utils.markdown_utils` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `IllustrationWriter` | class |
| 4 | `__init__` | function |
| 10 | `write_illustration` | function |

**Read in this order:**

- Make available: `ghost_writer.utils.markdown_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `IllustrationWriter`. Separate the class definition, constructing an instance, and calling its methods.
- `self.illustrator.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `self.transcriber.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/scene_writer.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/scene_writer.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `ghost_writer.models` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.writer_templates` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.transcribe_tool` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.utils.markdown_utils` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `SceneWriter` | class |
| 9 | `__init__` | function |
| 13 | `write_scene` | function |

**Read in this order:**

- Make available: `ghost_writer.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.transcribe_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.writer_templates`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.markdown_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `SceneWriter`. Separate the class definition, constructing an instance, and calling its methods.
- `self.transcriber.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `Task`: Describe assigned work and expected output.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/test_book_writer_service.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/test_book_writer_service.py)

**Imports:**

- `crewai` — Roles, tasks, crews, and workflow configuration.
- `ghost_writer.models` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.services.book_writer_service` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `mock_transcriber` | function |
| 13 | `mock_illustrator` | function |
| 17 | `mock_pdf_tool` | function |
| 21 | `author_agent` | function |
| 25 | `book_writer` | function |
| 35 | `test_write_act_calls_transcriber_and_increments_chapter` | function |
| 61 | `test_write_book_cover_runs_illustrator_and_transcriber` | function |
| 83 | `test_save_pdf_calls_pdf_tool` | function |

**Read in this order:**

- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.services.book_writer_service`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `mock_transcriber()`. Its body runs when called.
- Define `mock_illustrator()`. Its body runs when called.
- Define `mock_pdf_tool()`. Its body runs when called.
- Define `author_agent()`. Its body runs when called.
- Define `book_writer(tmp_path, author_agent, mock_transcriber, mock_illustrator, mock_pdf_tool)`. Its body runs when called.
- Define `test_write_act_calls_transcriber_and_increments_chapter(book_writer, mock_transcriber)`. Its body runs when called.
- Define `test_write_book_cover_runs_illustrator_and_transcriber(author_agent, mock_transcriber)`. Its body runs when called.
- Define `test_save_pdf_calls_pdf_tool(book_writer, mock_pdf_tool)`. Its body runs when called.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `book_writer.book_md_path.write_text`: Write a text file at the selected path.
- `call.kwargs.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/services/writer_templates.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/services/writer_templates.py)

**Imports:**

- `ghost_writer.models` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `get_scene_task_prompt` | function |
| 31 | `get_chapter_illustration_prompt` | function |
| 51 | `get_book_cover_illustration_prompt` | function |

**Read in this order:**

- Make available: `ghost_writer.models`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `get_scene_task_prompt(scene, act, chapter)`. Its body runs when called.
- Define `get_chapter_illustration_prompt(chapter, artistic_vision)`. Its body runs when called.
- Define `get_book_cover_illustration_prompt(book_info, artistic_vision)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/__init__.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/__init__.py)

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/convert_to_pdf_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/convert_to_pdf_tool.py)

**Imports:**

- `contextlib` — Resource lifetime helpers.
- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `markdown_pdf` — Local or third-party dependency; follow its definition/import before execution.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 11 | `pushd` | function |
| 21 | `MarkdownToPDFInput` | class |
| 25 | `MarkdownToPDFTool` | class |
| 30 | `_run` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `markdown_pdf`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `contextlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `pushd(directory)`. Its body runs when called.
- Define class `MarkdownToPDFInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MarkdownToPDFTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/custom_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/custom_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `MyCustomToolInput` | class |
| 10 | `MyCustomTool` | class |
| 17 | `_run` | function |

**Read in this order:**

- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `MyCustomToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `MyCustomTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/illustrator_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/illustrator_tool.py)

**Imports:**

- `base64` — Local or third-party dependency; follow its definition/import before execution.
- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `openai` — Compatible model clients and request/response types.
- `os` — Process configuration and operating-system paths.
- `pathlib` — Explicit filesystem paths.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 9 | `IllustratorToolInput` | class |
| 12 | `IllustratorTool` | class |
| 17 | `_run` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `base64`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `IllustratorToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `IllustratorTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `openai.OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/test_convert_to_pdf_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/test_convert_to_pdf_tool.py)

**Imports:**

- `PIL` — Image objects.
- `ghost_writer.tools.convert_to_pdf_tool` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `create_test_image` | function |
| 11 | `test_markdown_to_pdf_tool_with_image` | function |

**Read in this order:**

- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `PIL`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.convert_to_pdf_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `create_test_image(path)`. Its body runs when called.
- Define `test_markdown_to_pdf_tool_with_image(tmp_path)`. Its body runs when called.
- `md_path.write_text`: Write a text file at the selected path.
- `tool.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `print`: Show diagnostic output; this does not replace returning a callback result.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/test_illustrator_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/test_illustrator_tool.py)

**Imports:**

- `base64` — Local or third-party dependency; follow its definition/import before execution.
- `ghost_writer.tools.illustrator_tool` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.
- `unittest.mock` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 10 | `test_image_is_written` | function |
| 32 | `test_api_failure` | function |

**Read in this order:**

- Make available: `base64`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `unittest.mock`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.tools.illustrator_tool`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_image_is_written(tmp_path)`. Its body runs when called.
- Define `test_api_failure(tmp_path)`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/transcribe_tool.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/tools/transcribe_tool.py)

**Imports:**

- `crewai.tools` — Roles, tasks, crews, and workflow configuration.
- `pydantic` — Validated structured objects.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `TranscribeToolInput` | class |
| 8 | `TranscribeTool` | class |
| 14 | `_run` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `crewai.tools`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pydantic`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `TranscribeToolInput` using `BaseModel`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `TranscribeTool` using `BaseTool`. Separate the class definition, constructing an instance, and calling its methods.
- `Field`: Describe a schema field or constrain allowed values.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `file.write`: Write data to a file-like object; inspect the path and mode.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/filesystem_utils.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/filesystem_utils.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.
- `shutil` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 4 | `purge_directory` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `shutil`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `purge_directory(path)`. Its body runs when called.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/markdown_utils.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/markdown_utils.py)

**Imports:**

- `pathlib` — Explicit filesystem paths.

| Source line | Definition | Kind |
| --- | --- | --- |
| 5 | `add_page_break` | function |
| 11 | `image_markdown` | function |
| 28 | `header_markdown` | function |
| 41 | `write_markdown` | function |
| 54 | `code_block_markdown` | function |

**Read in this order:**

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `PAGE_BREAK`. Re-executing this cell updates kernel state.
- Define `add_page_break()`. Its body runs when called.
- Define `image_markdown(image_path, alt_text)`. Its body runs when called.
- Define `header_markdown(text, level)`. Its body runs when called.
- Define `write_markdown(content, file_path, mode)`. Its body runs when called.
- Define `code_block_markdown(code, language)`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/test_filesystem_utils.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/test_filesystem_utils.py)

**Imports:**

- `ghost_writer.utils.filesystem_utils` — Local or third-party dependency; follow its definition/import before execution.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 6 | `test_dir` | function |
| 21 | `test_purge_directory_removes_contents_and_recreates` | function |

**Read in this order:**

- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.filesystem_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_dir(tmp_path)`. Its body runs when called.
- Define `test_purge_directory_removes_contents_and_recreates(test_dir)`. Its body runs when called.
- `.write_text`: Write a text file at the selected path.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/test_markdown_utils.py](../../../../../../3_crew/community_contributions/ghost_writer/src/ghost_writer/utils/test_markdown_utils.py)

**Imports:**

- `ghost_writer.utils.markdown_utils` — Local or third-party dependency; follow its definition/import before execution.
- `pathlib` — Explicit filesystem paths.
- `pytest` — Local or third-party dependency; follow its definition/import before execution.
- `tempfile` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 8 | `test_add_page_break` | function |
| 12 | `test_image_markdown` | function |
| 16 | `test_header_markdown` | function |
| 20 | `test_code_block_markdown` | function |
| 27 | `test_write_markdown` | function |

**Read in this order:**

- Make available: `tempfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pytest`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `ghost_writer.utils.markdown_utils`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `test_add_page_break()`. Its body runs when called.
- Define `test_image_markdown()`. Its body runs when called.
- Define `test_header_markdown()`. Its body runs when called.
- Define `test_code_block_markdown()`. Its body runs when called.
- Define `test_write_markdown(tmp_path)`. Its body runs when called.
- `open`: Open a file or resource. Inspect the mode and path before running it.
- `f.read`: Read content from a file-like object. Check the receiver and its lifetime.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
