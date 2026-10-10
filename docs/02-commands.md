# Command cheat sheet

Sources: [command line guide](../guides/02_command_line.ipynb), [Git guide](../guides/03_git_and_github.ipynb), [Windows setup](../setup/SETUP-PC.md), [root README](../README.md), [Node setup](../setup/SETUP-node.md).

## PowerShell navigation

Run these in a terminal. `YOUR_PROJECT` and package names below are values you replace.

| Command | Purpose |
| --- | --- |
| `pwd` | Show the current directory |
| `ls` | List files and folders |
| `cd YOUR_PROJECT` | Move into a directory |
| `cd ..` | Move up one directory |
| `mkdir my_project` | Create a folder |
| `Get-Content README.md` | Read a text file |

## uv

| Command | Purpose / where to run |
| --- | --- |
| `uv sync` | Install the root project's declared environment |
| `uv run python my_script.py` | Run your script in its project environment |
| `uv add requests` | Add a dependency and update project metadata / lock |
| `uv python list` | Inspect available Python versions |
| `uv tool list` | Inspect standalone tools installed through uv |
| `uv tool install crewai` | Install the CrewAI CLI as a standalone tool |
| `uv tool upgrade crewai` | Upgrade that standalone CLI |
| `uv self update` | Update uv itself, as described in course setup |
| `uv lock --upgrade` | Upgrade locked dependencies; changes the lockfile |

Adding or upgrading packages changes your environment. Use `uv sync` with the existing course lock before deciding to upgrade.

## CrewAI and browser tools

```powershell
uv tool install crewai
cd 3_crew\debate
crewai install
crewai run
```

Run CrewAI commands inside a crew project, which has its own `pyproject.toml` and `src/`. The crew README describes configuration. To create a separate practice crew, the course uses `crewai create crew my_crew`.

If the Windows CrewAI creation command raises the Unicode error described in the root README, its workaround is:

```powershell
$env:PYTHONUTF8 = "1"
```

```powershell
node --version
npx --version
uv run playwright install
ollama pull llama3.2
```

The Ollama command downloads the model used in the course. Ollama must already be installed and running for local model calls.

## Git basics

```powershell
git status
git diff
git add docs
git commit -m "Add learning notes"
git push
```

`git status` shows changed files; `git diff` shows unstaged edits; `git add` selects what goes into the next commit. Check `git diff --cached` before committing. Git commands can operate anywhere within the repository.

## Notebook versus terminal

```python
!node --version
```

That leading `!` runs a shell command from a notebook. In PowerShell use `node --version`. Python `import`, `await`, and `os.environ[...]` belong in Python, not directly in the terminal. Shell environment changes made by a notebook subprocess do not update the already-running Python kernel.
