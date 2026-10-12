# Build and check small pieces

Source: [guides/07_vibe_coding_and_debugging.ipynb](../../../../guides/07_vibe_coding_and_debugging.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Describe one small behavior → generate/propose a small implementation → reproduce a case → test it → extend incrementally.

**Before source execution:** Generated code must be inspected and verified; receiving it is not evidence it works. Practice parses harmless text rather than executing generated programs.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Evaluate a small prototype with a fixed dataset

Choose a success measure before comparing approaches. Use a small curated set so you can reproduce failures. Scores below describe fictional fixtures, not real model performance.

**Ask yourself:** Which fixture scores better, and what does the score actually measure?

**Practice:** Add a case where a correct paraphrase differs from the expected string. Explain why exact-match scoring may be insufficient.

### Reproduce, inspect, simplify, fix

A small failing case separates causes. Catch the exact exception in a teaching example so the notebook can continue, then implement a specific repair.

**Ask yourself:** Will the original operation raise NameError or KeyError?

**Practice:** Remove the record definition in a scratch cell. Explain how the new NameError differs from the original missing-key error.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

## Follow the source in order

### Cell 1: Vibe coding and debugging

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Explanation, context, or transition

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: Vibe Coding for a larger project

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: The golden rule: start small, work incrementally!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: blank separator

No execution needed.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
