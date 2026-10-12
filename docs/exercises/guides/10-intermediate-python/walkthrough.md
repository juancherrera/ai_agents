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

## Instructor code, included here

These are the code examples from the local guide, in their original order, so you can compare the instructor's syntax with the explanations above without opening another file. These excerpts can depend on earlier definitions. The complete teaching examples above include their own inputs.

Source examples that intentionally fail, run indefinitely, download software, or contact a service are labeled below. Hosted configuration reads have been changed to your explicit `.environment/.env` path; credential values are never included.

### A briefing on more advanced features of Python

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# First let's create some things:

fruits = ["Apples", "Bananas", "Pears"]

book1 = {"title": "Great Expectations", "author": "Charles Dickens"}
book2 = {"title": "Bleak House", "author": "Charles Dickens"}
book3 = {"title": "An Book By No Author"}
book4 = {"title": "Moby Dick", "author": "Herman Melville"}

books = [book1, book2, book3, book4]
```

### Part 1: List and dict comprehensions

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Simple enough to start

for fruit in fruits:
    print(fruit)
```

### Part 1: List and dict comprehensions

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Let's make a new version of fruits

fruits_shouted = []
for fruit in fruits:
    fruits_shouted.append(fruit.upper())

fruits_shouted
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# You probably already know this
# There's a nice Python construct called "list comprehension" that does this:

fruits_shouted2 = [fruit.upper() for fruit in fruits]
fruits_shouted2
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# But you may not know that you can do this to create dictionaries, too:

fruit_mapping = {fruit: fruit.upper() for fruit in fruits}
fruit_mapping
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# you can also use the if statement to filter the results

fruits_with_longer_names_shouted = [fruit.upper() for fruit in fruits if len(fruit)>5]
fruits_with_longer_names_shouted
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
fruit_mapping_unless_starts_with_a = {fruit: fruit.upper() for fruit in fruits if not fruit.startswith('A')}
fruit_mapping_unless_starts_with_a
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# Another comprehension

[book['title'] for book in books]
```

### Part 1: List and dict comprehensions

This intentionally raises KeyError when a book has no author. Use the .get example that follows to handle missing keys.

```python
# This code will fail with an error because one of our books doesn't have an author

[book['author'] for book in books]
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# But this will work, because get() returns None

[book.get('author') for book in books]
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# And this variation will filter out the None

[book.get('author') for book in books if book.get('author')]
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# And this version will convert it into a set, removing duplicates

set([book.get('author') for book in books if book.get('author')])
```

### Part 1: List and dict comprehensions

Follow the for clause to find the input items, then read the expression before it to see what each item becomes. An if clause filters items.

```python
# And finally, this version is even nicer
# curly braces creates a set, so this is a set comprehension

{book.get('author') for book in books if book.get('author')}
```

### Part 2: Generators

yield supplies the next item and pauses; yield from supplies each item from the following iterable.

```python
# First define a generator; it looks like a function, but it has yield instead of return

import time

def come_up_with_fruit_names():
    for fruit in fruits:
        time.sleep(1) # thinking of a fruit
        yield fruit
```

### Part 2: Generators

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Then use it

for fruit in come_up_with_fruit_names():
    print(fruit)
```

### Part 2: Generators

yield supplies the next item and pauses; yield from supplies each item from the following iterable.

```python
# Here's another one

def authors_generator():
    for book in books:
        if book.get("author"):
            yield book.get("author")
```

### Part 2: Generators

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Use it

for author in authors_generator():
    print(author)
```

### Part 2: Generators

yield supplies the next item and pauses; yield from supplies each item from the following iterable.

```python
# Here's the same thing written with list comprehension

def authors_generator():
    for author in [book.get("author") for book in books if book.get("author")]:
        yield author
```

### Part 2: Generators

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Use it

for author in authors_generator():
    print(author)
```

### Part 2: Generators

yield supplies the next item and pauses; yield from supplies each item from the following iterable.

```python
# Here's a nice shortcut
# You can use "yield from" to yield each item of an iterable

def authors_generator():
    yield from [book.get("author") for book in books if book.get("author")]
```

### Part 2: Generators

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Use it

for author in authors_generator():
    print(author)
```

### Part 2: Generators

yield supplies the next item and pauses; yield from supplies each item from the following iterable.

```python
# And finally - we can replace the list comprehension with a set comprehension

def unique_authors_generator():
    yield from {book.get("author") for book in books if book.get("author")}
```

### Part 2: Generators

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Use it

for author in unique_authors_generator():
    print(author)
```

### Part 2: Generators

This loop does not finish on its own. Read it to understand streaming; use the notebook stop button if you choose to run it.

```python
# And for some fun - press the stop button in the toolbar when bored!
# It's like we've made our own Large Language Model... although not particularly large..
# See if you understand why it prints a letter at a time, instead of a word at a time. If you're unsure, try removing the keyword "from" everywhere in the code.

import random
import time

pronouns = ["I", "You", "We", "They"]
verbs = ["eat", "detest", "bathe in", "deny the existence of", "resent", "pontificate about", "juggle", "impersonate", "worship", "misplace", "conspire with", "philosophize about", "tap dance on", "dramatically renounce", "secretly collect"]
adjectives = ["turqoise", "smelly", "arrogant", "festering", "pleasing", "whimsical", "disheveled", "pretentious", "wobbly", "melodramatic", "pompous", "fluorescent", "bewildered", "suspicious", "overripe"]
nouns = ["turnips", "rodents", "eels", "walruses", "kumquats", "monocles", "spreadsheets", "bagpipes", "wombats", "accordions", "mustaches", "calculators", "jellyfish", "thermostats"]

def infinite_random_sentences():
    while True:
        yield from random.choice(pronouns)
        yield " "
        yield from random.choice(verbs)
        yield " "
        yield from random.choice(adjectives)
        yield " "
        yield from random.choice(nouns)
        yield ". "

for letter in infinite_random_sentences():
    print(letter, end="", flush=True)
    time.sleep(0.02)
```

### Part 5: Docker

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
# You need to install docker to run this example
# This will download the Docker image for python 3.12, create a container,
# Run some python code and print the result

!docker run --rm python:3.12 python -c "print(2 + 2)"
```

---

Source: [10_intermediate_python.ipynb](../../../../guides/10_intermediate_python.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
