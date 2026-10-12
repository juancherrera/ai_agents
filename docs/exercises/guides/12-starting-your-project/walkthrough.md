# Define success and experiment incrementally

Source: [guides/12_starting_your_project.ipynb](../../../../guides/12_starting_your_project.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Choose a measurable outcome → curate examples → compare small prototypes → inspect failures → expand only after understanding the result.

**Before source execution:** Fictional prototype scores in practice are teaching fixtures, not measurements of any real model.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Evaluate a small prototype with a fixed dataset

Choose a success measure before comparing approaches. Use a small curated set so you can reproduce failures. Scores below describe fictional fixtures, not real model performance.

**Ask yourself:** Which fixture scores better, and what does the score actually measure?

**Practice:** Add a case where a correct paraphrase differs from the expected string. Explain why exact-match scoring may be insufficient.

### Chaining: one output becomes the next input

A chain has a fixed order. Each stage depends on the previous result. Here the replies are explicit fictional fixtures, so you can observe the wiring without an API.

**Ask yourself:** Does the second stage receive the first result, or just the original question?

**Practice:** Change choose_area to a different fictional business area. Trace the changed value through each stage.

### Reproduce, inspect, simplify, fix

A small failing case separates causes. Catch the exact exception in a teaching example so the notebook can continue, then implement a specific repair.

**Ask yourself:** Will the original operation raise NameError or KeyError?

**Practice:** Remove the record definition in a scratch cell. Explain how the new NameError differs from the original missing-key error.

## Follow the source in order

### Cell 1: How to start working on your idea

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Advice 1: Be a Scientist

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: Advice 2: Embrace R&D

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: Advice 3: Dream big but start SMALL!

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 5: In summary

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
