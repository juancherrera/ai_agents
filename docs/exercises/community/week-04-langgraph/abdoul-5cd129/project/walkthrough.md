# abdoul: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Variables, dictionaries, and functions

A function consumes an input and returns a value. Dictionary lookup with get can handle optional fields. Comprehensions transform a collection.

**Ask yourself:** Which book has no author?

**Practice:** Add another authored book. Collect unique authors with a set and explain how it differs from a list.

### Working directory vs. source directory

Relative paths start at the process working directory. They do not automatically start next to a notebook. This example inspects paths only and never creates or deletes files.

**Ask yourself:** What folder is the starting point for Path(".")?

**Practice:** Compare the path from the repository root with the path from a week folder. Explain why cd changes relative-file behavior.

### Evaluate a small prototype with a fixed dataset

Choose a success measure before comparing approaches. Use a small curated set so you can reproduce failures. Scores below describe fictional fixtures, not real model performance.

**Ask yourself:** Which fixture scores better, and what does the score actually measure?

**Practice:** Add a case where a correct paraphrase differs from the expected string. Explain why exact-match scoring may be insufficient.

## Configuration and supporting files

- [4_langgraph/community_contributions/abdoul/sandbox/latest_bbc_news.md](../../../../../../4_langgraph/community_contributions/abdoul/sandbox/latest_bbc_news.md): Latest BBC News Summary. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Reading exercise

This contribution supplies documents/configuration rather than Python modules. Restate its expected inputs and outputs; follow the linked notebook packs for executable examples.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
