# 12 · Turn an idea into a measurable first project

Start with one input, one output, and a definition of success. Build a small prototype, test representative cases, then change one approach and compare it on the same cases. This implements the source guide's advice to experiment like a scientist.

## Define a narrow job

Example: label a support message as `billing` or `other`. This is small enough to test before building a chat UI or an agent team. The fixture below is local example data, not customer data.

```python
cases = [
    {"text": "Please resend my invoice", "expected": "billing"},
    {"text": "How can I change my password?", "expected": "other"},
    {"text": "I need a refund", "expected": "billing"},
]

def classify(text):
    return "billing" if "invoice" in text.lower() else "other"

correct = 0
for case in cases:
    actual = classify(case["text"])
    passed = actual == case["expected"]
    correct += int(passed)
    print(actual, case["expected"], passed)
print(f"Passed {correct}/{len(cases)}")
```

Output:

```text
billing billing True
other other True
other billing False
Passed 2/3
```

The classifier is a deliberately simple baseline. The conditional expression chooses one of two labels. `int(True)` is `1` and `int(False)` is `0`, allowing the loop to count successes. The failure identifies a concrete gap: refunds do not mention invoices.

## Compare an improvement on the same cases

```python
cases = [
    ("Please resend my invoice", "billing"),
    ("How can I change my password?", "other"),
    ("I need a refund", "billing"),
]

def classify(text):
    words = text.lower().split()
    return "billing" if any(word in words for word in ["invoice", "refund"]) else "other"

results = [classify(text) == expected for text, expected in cases]
print(f"Passed {sum(results)}/{len(cases)}")
```

Output: `Passed 3/3`. Each tuple is unpacked into `text` and `expected`. `any` is true when at least one candidate matches. This proves the change passes these three cases; it does not establish performance on unseen messages.

## Find limitations before adding complexity

Try `"My invoice?"`: splitting retains punctuation, so `invoice?` does not match `invoice`. Try `"This is not a refund request"`: keyword presence ignores meaning. Record these as separate cases with the label you actually want. If you later substitute a model, keep the same interface and test cases so you can compare approaches fairly.

Before a real model prototype, define a request limit, expected output format, timeout, and how failures appear to the user. Keep private configuration in `.environment/.env`. Measure accuracy, response time, and usage separately; one successful demonstration does not measure reliability.

## Practice

Add five cases of your own before changing the classifier. Include punctuation, capitalization, a misleading keyword, and an empty input. Write expected labels first. Then make one improvement, rerun all cases, and keep both the successes and failures in your notes.

---

Source: [12_starting_your_project.ipynb](../../../../guides/12_starting_your_project.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
