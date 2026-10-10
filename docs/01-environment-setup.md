# Environment setup

Sources: [Windows setup](../setup/SETUP-PC.md), [macOS](../setup/SETUP-mac.md), [Linux](../setup/SETUP-linux.md), [WSL](../setup/SETUP-WSL.md), [Node and Playwright](../setup/SETUP-node.md), [project dependencies](../pyproject.toml).

## The pieces

| Piece | What it is for |
| --- | --- |
| Python | Runs your program; this project's manifest requires Python >=3.12 |
| uv | Manages Python environments, packages, scripts, and standalone tools |
| `.venv/` | This project's installed Python environment |
| `pyproject.toml` | Project metadata and declared dependencies |
| `uv.lock` | Locked dependency versions; change with uv commands |
| `.env` | Your local API configuration; excluded by the root `.gitignore` |
| Notebook kernel | The Python process that executes notebook cells |
| Node / npx | Used to launch the Node-based MCP servers in the course |

## First setup

Install Git and uv using the original platform setup guide, then open a terminal at the repository root. For this checkout on Windows:

```powershell
cd C:\src\github_pub\ai_agents
uv --version
uv sync
uv python list
uv run python --version
```

`uv sync` installs the project environment. `uv run` runs a command in that environment. The Windows course guide highlights long path support and Microsoft Build Tools for packages that require compilation; see that guide if an installation fails.

Open a course `.ipynb` in your editor, choose the project's `.venv` kernel, and run cells in order with Shift+Enter. A terminal interpreter and a notebook kernel can be different. Check the notebook's interpreter with:

```python
import sys
print(sys.executable)
```

## API configuration

Create `.env` in the project root. Replace the placeholders only in your local file, for the services you use:

```dotenv
OPENAI_API_KEY=<YOUR_OPENAI_KEY>
GOOGLE_API_KEY=<YOUR_GOOGLE_KEY>
ANTHROPIC_API_KEY=<YOUR_ANTHROPIC_KEY>
```

The course's CrewAI notes also use `GEMINI_API_KEY` for Gemini. Email, search, notifications, and market-data lessons use additional variables; find their names in the original lesson code.

```python
import os
from dotenv import load_dotenv

load_dotenv(override=True)
print("OpenAI key configured:", bool(os.getenv("OPENAI_API_KEY")))
```

`override=True` lets values found in `.env` replace existing environment variables. Check presence without printing a key. Do not copy your `.env` into the reference library or commit it.

## Optional browser prerequisites

The course's Node setup file specifies Node v22 or later; this is the course requirement, not a current compatibility assessment.

```powershell
node --version
npx --version
uv run playwright install
```

Linux / WSL course alternative:

```bash
uv run playwright install --with-deps chromium
```

Windows and WSL have separate environments. The WSL lesson explains installing Python tooling and Node inside WSL. Follow it when running the week-six exercises there.
