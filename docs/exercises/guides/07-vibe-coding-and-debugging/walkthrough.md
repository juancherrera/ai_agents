# 07 · Use AI-generated code in small, checkable pieces

The guide's useful habit is to ask for a small result, inspect it, and test it before expanding the program. Treat generated code as a proposed implementation. You still need to understand what inputs it accepts and how you will know it works.

## Specify behavior before asking for code

Here is a concrete prompt you can reuse:

> Write a Python function `normalize_tags(text)` using only the standard library. Split comma-separated tags, trim outer whitespace, lowercase each tag, discard empty tags, and preserve duplicates and order. Show a short implementation and tests. Do not add a server, database, or API call. Explain each Python construct.

This gives the assistant boundaries and observable outcomes. Compare the returned code with this complete implementation:

```python
def normalize_tags(text):
    tags = []
    for part in text.split(","):
        tag = part.strip().lower()
        if tag:  # Empty strings are false in a condition.
            tags.append(tag)
    return tags

assert normalize_tags(" Python, AI, ,Python ") == ["python", "ai", "python"]
assert normalize_tags("") == []
print("Checks passed")
```

Output: `Checks passed`. Splitting creates candidate strings. Cleaning happens once per candidate. The condition removes empty ones. Appending preserves order and duplicates. `assert` raises an error if the claimed behavior is false; it is useful for these local learning checks.

## Compare a shorter version without changing behavior

```python
def normalize_tags(text):
    return [part.strip().lower() for part in text.split(",") if part.strip()]

assert normalize_tags(" Python, AI, ,Python ") == ["python", "ai", "python"]
assert normalize_tags("") == []
print("Same results")
```

Output: `Same results`. The expression before `for` produces each output item. The expression after `if` decides whether to include it. Prefer the longer version while you are learning if it is easier to inspect. Fewer lines do not establish correctness.

## Expand only after a passing check

If you want duplicates removed, first decide whether order should remain. Then add a test such as `assert normalize_tags("AI,Python,AI") == ["ai", "python"]`. The current implementations fail that new requirement, which tells you exactly what needs to change.

Ask for one change at a time and keep the previous checks. When a generated answer imports an unfamiliar package or calls an external service, inspect why it needs that dependency before running it.

## Practice

Add a test for `" A , B "`. Expected list: `['a', 'b']`. Then deliberately change `.lower()` to `.upper()` and run the checks. The assertion failure demonstrates that the tests detect a real behavior change.

---

Source: [07_vibe_coding_and_debugging.ipynb](../../../../guides/07_vibe_coding_and_debugging.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
