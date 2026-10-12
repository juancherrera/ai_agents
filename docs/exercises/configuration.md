# Configuration and deliberate source execution

All practice notebooks default to offline exercises. They never read private environment files or automatically run the linked original source. A simulated result does not verify provider access, browser behavior, MCP transport, delivery, or deployment.

## Your credential location

Keep tokens in `C:\src\github_pub\ai_agents\.environment\.env`. Use variable names expected by the specific source. Never place token values in a script or notebook cell and never print values or prefixes. The file itself is plaintext; loading places its variables in process memory.

For a deliberate hosted exercise, use this in your own working copy before constructing clients:

```python
from pathlib import Path
from dotenv import load_dotenv
import os

root = Path(r"C:\src\github_pub\ai_agents")
env_path = root / ".environment" / ".env"
if not env_path.is_file():
    raise FileNotFoundError(f"Missing configuration file: {env_path}")
load_dotenv(env_path, override=True)
print("OpenAI configured:", bool(os.getenv("OPENAI_API_KEY")))
```

Replace the original source's bare `load_dotenv(override=True)` calls with explicit loading in your practice copy. A later bare call could otherwise load a different root `.env` and override settings. Existing instructor and community source files have not been changed.

## Separate environments

Most practice notebooks use the root `.venv` and ordinary Python; a few exercise the installed Pydantic, LangGraph, MCP, or Pillow libraries locally. Original CrewAI projects have their own manifests and environments. Follow the relevant project README; importing CrewAI from the root environment is not assumed to work.

## Before running original code

Read its imports and launcher. Find model requests, notification/email sends, filesystem/database writes, reset/delete operations, generated-code execution, listeners, and recurring loops. Replace external operations with fictional fixtures until the flow is understood. Use a selected practice directory/database and a finite run limit.

Notebook cell positions in the maps include Markdown and blank cells. Python line numbers refer to the extracted local version and may shift when the source changes. Restart the kernel before testing a fresh top-to-bottom sequence; an old function definition can hide missing imports or source errors.

## Evidence boundaries

- Extracted cell/module maps describe local source structure, not an execution trace.
- Automated verification covers the authored offline exercises, not original model outputs.
- Provider model identifiers, compatibility, pricing, external linked tutorials, and source deployment behavior are UNVERIFIED unless checked separately.
- Saved source notebook outputs and profile/customer/credential data are not copied into these exercise packs.
