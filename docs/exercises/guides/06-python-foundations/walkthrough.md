# 06 · Python foundations for someone who already codes

Python uses indentation for blocks, `def` for functions, and objects such as lists and dictionaries for everyday data. Run the examples independently; each includes its inputs.

## Functions, defaults, and formatted strings

```python
def greet(name, greeting="Hello"):
    # f before the quote allows expressions inside {braces}.
    return f"{greeting}, {name}!"

print(greet("JC"))
print(greet("JC", greeting="Welcome"))
```

Output: `Hello, JC!` and `Welcome, JC!`. The default is used when the caller omits `greeting`. A keyword argument names the parameter explicitly. Use functions to give a repeated transformation a name and test it separately.

## Strings: clean, split, select, and join

```python
text = "  apple,pear,banana  "
clean = text.strip()            # Remove outer whitespace.
parts = clean.split(",")       # Produce a list of three strings.
print(parts[0])                 # First item.
print(parts[1:])                # Slice from index 1 through the end.
print(" | ".join(parts))        # Put a separator between items.
print(clean.replace("pear", "plum"))
```

Output:

```text
apple
['pear', 'banana']
apple | pear | banana
apple,plum,banana
```

Slices use `[start:stop]`; the stop index is excluded. Strings are immutable: these methods return new strings instead of editing `text`. This matters when cleaning input before sending it to a model.

## Lists, dictionaries, and sets

```python
names = ["Ada", "Linus", "Ada"]
names.append("Grace")
profile = {"name": "Ada", "language": "Python"}
print(names[0])
print(profile["language"])
print(profile.get("city", "Unknown"))
print(sorted(set(names)))
```

Output: `Ada`, `Python`, `Unknown`, and `['Ada', 'Grace', 'Linus']`. A list keeps ordered items and duplicates. A dictionary associates keys with values. `.get` supplies a fallback for an absent key. A set removes duplicates; `sorted` gives the demonstration a predictable display order.

## Read and write a file with automatic cleanup

```python
from pathlib import Path
from tempfile import TemporaryDirectory

with TemporaryDirectory() as folder:
    path = Path(folder) / "note.txt"
    path.write_text("Practice, then explain.", encoding="utf-8")
    print(path.read_text(encoding="utf-8"))
```

Output: `Practice, then explain.` `with` manages a resource; here the temporary folder is removed when the block ends. `/` combines `Path` components. Explicit UTF-8 avoids relying on the machine's default encoding.

## Classes keep related data and behavior together

```python
class Book:
    def __init__(self, title):
        self.title = title  # Store data on this particular object.

    def describe(self):
        return f"Book: {self.title}"

book = Book("Moby Dick")
print(book.describe())
```

Output: `Book: Moby Dick`. `__init__` initializes an instance. `self` refers to that instance; Python supplies it when you call `book.describe()`. `self.title` is an instance field, unlike a temporary local variable named `title`.

## Practice

Give `Book` an `author` parameter and store it in `self.author`. Change `describe` to include the author. For `Book("Moby Dick", "Herman Melville")`, aim for `Moby Dick by Herman Melville`.

---

Source: [06_python_foundations.ipynb](../../../../guides/06_python_foundations.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
