# Python building blocks and NameErrors

Source: [guides/06_python_foundations.ipynb](../../../../guides/06_python_foundations.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Understand values/functions → distinguish collection types → identify what defines a name → reproduce and fix missing definitions.

**Before source execution:** The source mainly links external tutorials; those pages are unverified. These local exercises supply runnable practice for the named concepts.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Variables, dictionaries, and functions

A function consumes an input and returns a value. Dictionary lookup with get can handle optional fields. Comprehensions transform a collection.

**Ask yourself:** Which book has no author?

**Practice:** Add another authored book. Collect unique authors with a set and explain how it differs from a list.

### Classes, methods, and generators

A class bundles data and behavior. A method receives self, the current instance. A generator yields values as they are requested.

**Ask yourself:** Will the same author be yielded twice?

**Practice:** Add a book by another author. Explain why yield allows iteration to continue while return would end it.

### Reproduce, inspect, simplify, fix

A small failing case separates causes. Catch the exact exception in a teaching example so the notebook can continue, then implement a specific repair.

**Ask yourself:** Will the original operation raise NameError or KeyError?

**Practice:** Remove the record definition in a scratch cell. Explain how the new NameError differs from the original missing-key error.

## Follow the source in order

### Cell 1: Foundations of Python and fixing NameErrors

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Next: a comprehensive briefing on Python

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: With this in mind - understanding NameErrors in Python

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 4: blank separator

No execution needed.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../configuration.md)
