# 03 · Understand Git and GitHub by making a change

Git records local snapshots of files. GitHub hosts repositories so people can share those snapshots. The workflow is: edit → inspect → stage the intended files → commit → push. A commit is local until you push it.

## Inspect the course repository

Run in PowerShell:

```powershell
Set-Location 'C:\src\github_pub\ai_agents'
git status --short
git branch --show-current
git remote -v
git diff --stat
```

`status` lists changed files; `M` means modified and `??` means untracked. `branch` tells you which line of work you are on. `remote` shows where fetch/push connect. `diff --stat` summarizes unstaged edits. These commands inspect state without changing your files.

## Practice the full local workflow in a fresh repository

Run these commands together. They create a unique temporary folder and configure an identity only for that practice repository.

```powershell
$gitLesson = Join-Path ([System.IO.Path]::GetTempPath()) ('git-lesson-' + [guid]::NewGuid())
New-Item -ItemType Directory -Path $gitLesson | Out-Null
Set-Location $gitLesson
git init -b main
git config user.name 'Practice Student'
git config user.email 'student@example.invalid'
'First version' | Set-Content -LiteralPath 'notes.txt'
git status --short
git add notes.txt
git diff --cached
git commit -m 'Add first practice note'
git log --oneline -1
```

Before `add`, `notes.txt` is untracked. `add` places its current contents in the staging area: the proposed next snapshot. `diff --cached` lets you review that proposal. `commit` saves it with a message. The log shows a generated commit ID and `Add first practice note`; your ID will differ.

## Make a second version on a branch

Continue in the same temporary repository:

```powershell
git switch -c practice-change
'Second version' | Set-Content -LiteralPath 'notes.txt'
git diff
git add notes.txt
git commit -m 'Revise practice note'
git switch main
Get-Content notes.txt
git merge --ff-only practice-change
Get-Content notes.txt
```

The branch starts at the existing commit. Editing and committing moves `practice-change` forward. Switching to `main` restores `First version`. The fast-forward merge moves `main` to the newer commit; the file now contains `Second version`. `--ff-only` refuses a merge requiring a new merge commit, so you can investigate diverged history first.

## Fetch, pull, and push in a real shared repository

These are the command forms; use your actual branch name and review local changes first:

```powershell
git fetch origin
git log --oneline --max-count=5 origin/main
```

`fetch` downloads remote history without updating your working files. When you are on a clean `main`, `git pull --ff-only origin main` fetches and advances it if history allows. To share a committed feature branch, use `git push -u origin your-branch-name`; substitute a real branch name. A GitHub pull request then asks the repository owner to review and incorporate that branch. Pushing your branch does not itself create a pull request.

## Practice

Create a third version in the temporary repository. Before committing, run both `git diff` and `git diff --cached`. Stage the file and run both again. The change moves from the first display to the second because staging chooses what the commit will contain.

---

Source: [03_git_and_github.ipynb](../../../../guides/03_git_and_github.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
