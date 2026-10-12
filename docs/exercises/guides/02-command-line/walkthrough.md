# 02 · Use the Windows terminal

A terminal runs commands in a current folder. Python code belongs in a Python file or notebook; the blocks labeled PowerShell below belong in the terminal. Use this when opening the repository, running scripts, or inspecting your environment.

## Find your location and open the repository

```powershell
# Show the current folder.
Get-Location
# Move to the course repository. Quoting also works for paths with spaces.
Set-Location 'C:\src\github_pub\ai_agents'
# List its files and folders.
Get-ChildItem
```

`Set-Location` changes where subsequent relative paths start. After it runs, `guides` means `C:\src\github_pub\ai_agents\guides`. `Get-ChildItem` should show the repository's folders, including `guides` and `1_foundations`.

## Create and run a disposable script

This uses a new temporary folder, so you can experiment without editing course files.

```powershell
$lessonFolder = Join-Path ([System.IO.Path]::GetTempPath()) ('ai-lesson-' + [guid]::NewGuid())
New-Item -ItemType Directory -Path $lessonFolder | Out-Null
Set-Location $lessonFolder
'print("Hello from Python")' | Set-Content -LiteralPath 'hello.py'
& 'C:\src\github_pub\ai_agents\.venv\Scripts\python.exe' hello.py
Get-Content -LiteralPath 'hello.py'
```

You should see `Hello from Python`, then the file's source. `$lessonFolder` is a PowerShell variable. `Join-Path` builds a path. `|` passes one command's output into the next command. `&` runs the quoted executable path. The course's `.venv` contains its Python environment; using its full path selects that interpreter explicitly.

## Diagnose a missing command

```powershell
Get-Command python -ErrorAction SilentlyContinue
& 'C:\src\github_pub\ai_agents\.venv\Scripts\python.exe' --version
Set-Location 'C:\src\github_pub\ai_agents'
```

The first command reports a `python` found through PATH, if one exists. The second identifies the course interpreter even if PATH has no Python. If the full path is missing, the repository environment has not been created at that location.

## Practice

In the temporary folder, change the script to `print(6 * 7)` with `Set-Content`, then run it again. Expected result: `42`. Use the up arrow to recall a previous command; press Ctrl+C to interrupt a running command.

---

Source: [02_command_line.ipynb](../../../../guides/02_command_line.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
