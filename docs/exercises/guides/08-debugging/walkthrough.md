# 08 · Debug by finding the first wrong assumption

Start with an exact input and an exact failure. Reduce the program until the failure fits in a small example. Then check the data at the failing operation. This is especially useful when a long agent workflow obscures a simple Python mistake.

## Reproduce an absent dictionary key

```python
book = {"title": "Unknown author"}
try:
    print(book["author"])
except KeyError as error:
    print(type(error).__name__)
    print("Available keys:", list(book))
```

Output: `KeyError`, then `Available keys: ['title']`. The problem is the assumption that every book has an author. The exception is caught here so you can inspect it without stopping the exercise.

Use a fallback only if a missing author is allowed:

```python
book = {"title": "Unknown author"}
print(book.get("author", "Not supplied"))
```

Output: `Not supplied`. If an author is mandatory, the appropriate fix is validation or correcting the input, rather than quietly supplying a fallback.

## Check a value's type before using it

```python
count = "3"  # Text, perhaps received from a form.
print(type(count).__name__)
number = int(count)
print(number + 1)
```

Output: `str`, then `4`. Adding an integer directly to `"3"` would raise `TypeError`. `int` converts valid integer text; `int("three")` instead raises `ValueError`. These errors point to different assumptions: incompatible types versus invalid contents.

## Read a traceback from the failing operation outward

A traceback names the exception and shows the calls leading to it. Locate the last relevant line in your own code, inspect its inputs, and check the caller that supplied them. A `NameError` means a name is unavailable in the current scope, often from spelling or notebook execution order. A `ModuleNotFoundError` means the running interpreter cannot import that module.

For an import problem, inspect the interpreter:

```python
import sys
print(sys.executable)
```

Expected result is the path of the interpreter actually running the example. Compare it with the environment where you installed the package.

## Write the regression check after the fix

```python
def author_label(book):
    return book.get("author", "Not supplied")

assert author_label({"title": "Unknown"}) == "Not supplied"
assert author_label({"author": "Dickens"}) == "Dickens"
print("Both cases passed")
```

Output: `Both cases passed`. The missing-field case prevents the original mistake from returning; the normal case ensures the fix did not break existing behavior.

## Practice

What happens with `{"author": None}`? `.get` returns `None`, because the key exists. If your requirement says null should also use the fallback, implement `book.get("author") or "Not supplied"` and add a check for it. That expression also treats an empty string as missing; decide whether that matches your requirements.

---

Source: [08_debugging.ipynb](../../../../guides/08_debugging.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
