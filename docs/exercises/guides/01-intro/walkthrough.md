# Choose a learning route

Source: [guides/01_intro.ipynb](../../../../guides/01_intro.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Identify prerequisites → choose a guide → practice one concept → return to its course lesson.

**Before source execution:** This notebook is a local guide index. External linked conversations are not extracted or verified by this pack.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Evaluate a small prototype with a fixed dataset

Choose a success measure before comparing approaches. Use a small curated set so you can reproduce failures. Scores below describe fictional fixtures, not real model performance.

**Ask yourself:** Which fixture scores better, and what does the score actually measure?

**Practice:** Add a case where a correct paraphrase differs from the expected string. Explain why exact-match scoring may be insufficient.

### Notebook state follows execution order

A kernel is a Python process with variables in memory. The order you execute cells determines the values. Restarting removes that state.

**Ask yourself:** Will changing fruit rebuild the previously formed string?

**Practice:** Split these statements into separate cells. Rerun them in different orders, then restart and run from the top.

## Follow the source in order

### Cell 1: Welcome to the Guides!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Introduction

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: Contents (select from the sub-directory in the explorer on the left)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
