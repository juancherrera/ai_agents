# Working changes, staging, commits, and remotes

Source: [guides/03_git_and_github.ipynb](../../../../guides/03_git_and_github.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Inspect differences → select changes → inspect staged content → record snapshot → synchronize deliberate commits.

**Before source execution:** External linked Git briefings are unverified; practice does not mutate Git history.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Inspect a snapshot before recording changes

Git distinguishes working files, staged changes, and committed snapshots. This model teaches the distinction without modifying repository history.

**Ask yourself:** Which changes are recorded when only notes.md is staged?

**Practice:** Use git status and git diff in PowerShell to inspect real state. Stage only files you intentionally want in a commit.

### Working directory vs. source directory

Relative paths start at the process working directory. They do not automatically start next to a notebook. This example inspects paths only and never creates or deletes files.

**Ask yourself:** What folder is the starting point for Path(".")?

**Practice:** Compare the path from the repository root with the path from a week folder. Explain why cd changes relative-file behavior.

## Follow the source in order

### Cell 1: Git and Github

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: Foundational briefing

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: Pulling latest code

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: Contributing your changes to the repo to share your contributions with others

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 6: If you'd like to become a Git pro

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
