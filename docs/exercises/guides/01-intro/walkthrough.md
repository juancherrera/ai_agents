# 01 · How to study this course

The course builds programs that send requests to models, use tools, and check results. Learn one piece at a time: understand the input, follow the transformation, then inspect the output. You already know programming; the new parts are Python's syntax and the model/tool workflow.

## Start with a small, visible transformation

Run this whole block in a notebook or save it as `lesson.py` and run it with Python:

```python
def make_message(question):
    # A dictionary stores named fields, like an object/map in other languages.
    return {"role": "user", "content": question}

message = make_message("What is 2 + 2?")
print(message["role"])
print(message["content"])
```

Output:

```text
user
What is 2 + 2?
```

`def` declares a function. The colon starts its body; indentation determines which statements belong to it. `return` sends the dictionary back to the caller. Square brackets retrieve a field. This is the kind of message later passed to a model; this example only constructs data, so no model or token is needed.

## Use the same study loop on each example

1. Predict the printed result before running it.
2. Run the complete example.
3. Change one input and run again.
4. Explain why the output changed in your own words.
5. Rebuild the example without looking at the answer.

If an example fails, keep the exact error and the smallest code that produces it. That gives you something concrete to investigate instead of replaying a whole lecture.

## Practice

Change the question to `Summarize this invoice`. Keep the role as `user`. The second printed line should be your new question. Then add `print(type(message).__name__)`; the result is `dict`.

The guide order is setup → Python → debugging → model connections → intermediate Python → async → your own project. Return to a topic when the course starts using it; you do not have to memorize every feature first.

---

Source: [01_intro.ipynb](../../../../guides/01_intro.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
