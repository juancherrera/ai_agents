# Model APIs, messages, and tools

Sources: [first API call](../1_foundations/1_lab1.ipynb), [multiple providers](../1_foundations/2_lab2.ipynb), [tools](../1_foundations/4_lab4.ipynb), [agent loop](../1_foundations/5_extra.ipynb), [API and Ollama guide](../guides/09_ai_apis_and_ollama.ipynb).

## A small hosted-model example

Requires the project environment and your configured API key. The model name is taken from course examples; availability has not been checked.

```python
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI()
messages = [
    {"role": "system", "content": "Explain Python simply."},
    {"role": "user", "content": "What does a dictionary do?"},
]
response = client.chat.completions.create(
    model="gpt-4o-mini", messages=messages
)
print(response.choices[0].message.content)
```

`system` gives instructions, `user` contains the request, and `assistant` contains model replies. To continue a conversation, send the prior messages along with the next request. The course's UI callbacks build this history.

## Local Ollama adaptation

First install/run Ollama and download the course model with `ollama pull llama3.2`.

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
response = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Explain an AI agent in one sentence."}],
)
print(response.choices[0].message.content)
```

Here `ollama` is the local endpoint's placeholder key. The course also demonstrates OpenAI-compatible endpoints for other providers: the URL, model, and credentials must all match the provider. An OpenAI-compatible chat API does not imply every OpenAI hosted tool or feature is available.

## The manual tool loop

The foundations lesson implements these steps:

1. Define Python functions and JSON schemas describing their arguments.
2. Send the schemas in the model call's `tools` argument.
3. If the response has tool calls, append the assistant message containing them.
4. Dispatch only recognized function names and decode arguments with `json.loads`.
5. Append each result with role `tool` and its matching `tool_call_id`.
6. Call the model again until it returns an answer.

The application executes tools. A model returning a tool request does not itself execute your Python function. Check the arguments before a tool performs an action. Put limits on loops when adapting the lesson for your own projects.

The [OpenAI Agents SDK reference](06-openai-agents-sdk.md) demonstrates the decorator that removes much of this boilerplate.
