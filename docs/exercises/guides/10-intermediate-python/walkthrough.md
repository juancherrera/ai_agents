# 10 · Comprehensions, generators, classes, and decorators

These features make the course's short Python expressions easier to read. The examples below build from ordinary loops to their compact equivalents and then solve the source guide's book exercise.

## A comprehension is a loop that builds a collection

```python
fruits = ["Apples", "Bananas", "Pears"]
shouted = []
for fruit in fruits:
    shouted.append(fruit.upper())

compact = [fruit.upper() for fruit in fruits]
assert compact == shouted
print(compact)
print([fruit.upper() for fruit in fruits if len(fruit) > 5])
print({fruit: fruit.upper() for fruit in fruits})
```

Output:

```text
['APPLES', 'BANANAS', 'PEARS']
['APPLES', 'BANANAS']
{'Apples': 'APPLES', 'Bananas': 'BANANAS', 'Pears': 'PEARS'}
```

Read `[result for item in collection if condition]` as: loop over the collection, keep items meeting the condition, and append each result. Braces with `key: value` produce a dictionary.

## Handle missing authors and remove duplicates

```python
books = [
    {"title": "Great Expectations", "author": "Charles Dickens"},
    {"title": "Bleak House", "author": "Charles Dickens"},
    {"title": "A Book By No Author"},
    {"title": "Moby Dick", "author": "Herman Melville"},
]
authors = {book.get("author") for book in books if book.get("author")}
print(sorted(authors))
```

Output: `['Charles Dickens', 'Herman Melville']`. `.get` returns `None` for the missing author. The condition skips that item. Braces without a colon create a set comprehension, so Dickens appears once. The source's `book['author']` would raise `KeyError` for the book without that key.

## A generator delivers one item at a time

```python
def fruit_names():
    for fruit in ["Apples", "Bananas", "Pears"]:
        yield fruit  # Pause here; resume when the caller asks for another item.

stream = fruit_names()
print(next(stream))
print(list(stream))
print(list(stream))
```

Output: `Apples`, then `['Bananas', 'Pears']`, then `[]`. Calling the generator function creates a generator; it does not immediately execute the loop. `next` requests one item. `list` consumes the remaining items. A consumed generator is exhausted, so create a new one to replay it. This is useful for streaming model output.

`yield` emits one object; `yield from` emits each item of an iterable:

```python
def whole_word():
    yield "Hi"

def letters():
    yield from "Hi"

print(list(whole_word()))
print(list(letters()))
```

Output: `['Hi']`, then `['H', 'i']`. A string is iterable by character. That explains the source's letter-by-letter sentence generator without needing its infinite loop.

## Solve the Book and BookShelf exercise

```python
class Book:
    def __init__(self, title: str, author: str | None = None):
        self.title = title
        self.author = author

    def has_author(self) -> bool:
        return bool(self.author)

class BookShelf:
    def __init__(self, books: list[Book]):
        self.books = books

    def unique_authors(self):
        seen = set()
        for book in self.books:
            if book.has_author() and book.author not in seen:
                seen.add(book.author)
                yield book.author

shelf = BookShelf([Book("One", "Dickens"), Book("Two", "Dickens"), Book("Three")])
print(list(shelf.unique_authors()))
```

Output: `['Dickens']`. `str | None` means text or no value; `-> bool` documents the return type. These hints do not enforce types at runtime. `seen` records authors already emitted; the generator preserves first-seen order while avoiding duplicates.

## Inheritance extends an existing class

```python
class Book:
    def __init__(self, title):
        self.title = title

class Ebook(Book):
    def __init__(self, title, format):
        super().__init__(title)  # Reuse the parent initialization.
        self.format = format

book = Ebook("Moby Dick", "epub")
print(book.title, book.format)
```

Output: `Moby Dick epub`. `Ebook(Book)` declares a subclass. `super` calls the parent implementation so you do not repeat its initialization.

## Pydantic checks data at runtime

```python
from pydantic import BaseModel, ValidationError

class Evaluation(BaseModel):
    acceptable: bool
    feedback: str

evaluation = Evaluation(acceptable=True, feedback="Clear answer")
print(evaluation.acceptable)
try:
    Evaluation(acceptable=True)  # Required feedback is missing.
except ValidationError:
    print("Missing feedback rejected")
```

Output: `True`, then `Missing feedback rejected`. Unlike plain annotations, constructing this model validates the input. The course uses this to represent structured evaluation results; successful validation does not prove the evaluator's judgment is correct.

## A decorator wraps a function

```python
from functools import wraps

def announce(function):
    @wraps(function)
    def wrapper(*args, **kwargs):
        print("Calling", function.__name__)
        return function(*args, **kwargs)
    return wrapper

@announce
def add(a, b):
    return a + b

print(add(2, 3))
```

Output: `Calling add`, then `5`. `@announce` is shorthand for replacing `add` with `announce(add)`. `*args` collects positional arguments; `**kwargs` collects named ones. The wrapper adds behavior and delegates the actual calculation. SDK decorators can similarly register a Python function as a tool.

## Docker runs Python in a container

With Docker installed and running, use this terminal command from the source:

```powershell
docker run --rm python:3.12 python -c "print(2 + 2)"
```

Expected program output: `4`, alongside possible image-download messages. `python:3.12` selects the image; the next `python` is the program inside the container; `-c` supplies its code; `--rm` removes the container after it exits. The downloaded image remains. Docker execution is not part of the verified offline Python exercises.

## Practice

Add `Book("Four", "Melville")` to the shelf. Expected authors: `['Dickens', 'Melville']`. Then explain why using `return book.author` inside the loop would stop the method after its first result, while `yield` allows it to continue.

---

Source: [10_intermediate_python.ipynb](../../../../guides/10_intermediate_python.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
