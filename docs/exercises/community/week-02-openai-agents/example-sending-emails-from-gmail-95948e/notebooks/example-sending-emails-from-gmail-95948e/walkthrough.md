# example sending emails from gmail

Source: [2_openai/community_contributions/example_sending_emails_from_gmail.ipynb](../../../../../../../2_openai/community_contributions/example_sending_emails_from_gmail.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Variables, dictionaries, and functions → Evaluate a small prototype with a fixed dataset

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Variables, dictionaries, and functions

A function consumes an input and returns a value. Dictionary lookup with get can handle optional fields. Comprehensions transform a collection.

**Ask yourself:** Which book has no author?

**Practice:** Add another authored book. Collect unique authors with a set and explain how it differs from a list.

### Evaluate a small prototype with a fixed dataset

Choose a success measure before comparing approaches. Use a small curated set so you can reproduce failures. Scores below describe fictional fixtures, not real model performance.

**Ask yourself:** Which fixture scores better, and what does the score actually measure?

**Practice:** Add a case where a correct paraphrase differs from the expected string. Explain why exact-match scoring may be insufficient.

## Follow the source in order

### Cell 1: Enable "App Passwords" in Gmail (Recommended for Security)

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 2: Notes

Read this before the next code cell. If it contains an assignment, restate the expected result before writing code. External pages linked here have not been extracted.

### Cell 3: code

- Make available: `smtplib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `email.mime.text`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `gmail_user`. Re-executing this cell updates kernel state.
- Store expression result in `gmail_app_password`. Re-executing this cell updates kernel state.
- Store expression result in `to`. Re-executing this cell updates kernel state.
- Store expression result in `subject`. Re-executing this cell updates kernel state.
- Store expression result in `body`. Re-executing this cell updates kernel state.
- Store expression result in `msg`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `server.send_message`: Deliver a message to an agent identity through a runtime.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
