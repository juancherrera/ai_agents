# Python patterns used in the course

Sources: [foundations](../guides/06_python_foundations.ipynb), [intermediate Python](../guides/10_intermediate_python.ipynb), [foundations tool lesson](../1_foundations/4_lab4.ipynb), [AutoGen messages](../5_autogen/messages.py).

## Lists, dictionaries, and comprehensions

```python
books = [{"title": "Python", "author": "Ada"}, {"title": "Agents"}]
titles = [book["title"] for book in books]
authors = {book.get("author") for book in books if book.get("author")}
print(titles)  # ['Python', 'Agents']
print(authors)  # {'Ada'}
```

Use `item["key"]` when the key must exist. Use `item.get("key")` when it is optional; a missing key returns `None`. A list preserves order; a set removes duplicates. Comprehensions collect or filter values.

## Functions and type hints

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

print(greet("Ada"))  # Hello, Ada!
```

Hints describe intended input and output types. They also help agent frameworks build tool schemas; ordinary Python hints alone do not validate inputs at runtime. A tool's docstring tells the model what the function does.

## JSON

```python
import json

encoded = json.dumps({"name": "Ada", "active": True})
decoded = json.loads(encoded)
print(decoded["name"])  # Ada
```

`dumps` converts Python data to JSON text. `loads` parses JSON text back into Python data. The foundations tool loop uses `json.loads` to decode model-generated tool arguments.

## Generators

```python
def words():
    yield "Learning"
    yield "agents"

print(list(words()))  # ['Learning', 'agents']
```

`return` ends a function with a result. `yield` emits one item while allowing later iteration to continue. Streaming UI callbacks use this pattern to return partial progress.

## Classes and structured data

```python
from dataclasses import dataclass

@dataclass
class Message:
    content: str

message = Message(content="Hello")
print(message.content)
```

AutoGen Core uses message objects for communication. Classes also bundle state and behavior in the course's Sidekick and account modules.

Pydantic schemas validate structured data and describe requested model outputs:

```python
from pydantic import BaseModel, Field

class Answer(BaseModel):
    summary: str = Field(description="A short explanation")
    confidence: float = Field(ge=0, le=1)

answer = Answer(summary="Tools let agents call Python functions.", confidence=0.9)
print(answer.summary)
```

A valid schema checks shape and constraints. It does not establish that the model's answer is true.
