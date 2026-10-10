# Local Ollama commands and examples

Sources: [multi-model lab](../../1_foundations/2_lab2.ipynb), [API and Ollama guide](../../guides/09_ai_apis_and_ollama.ipynb).

## Terminal commands

Install Ollama using the course guide before running these commands.

| Command | Purpose |
| --- | --- |
| `ollama serve` | Start the service if it is not already running |
| `ollama pull llama3.2` | Download the course's local model |
| `ollama ls` | List downloaded models |
| `ollama rm llama3.2` | Delete that downloaded model when you no longer need it |

The model download uses disk space. The course's local endpoint is `http://localhost:11434/v1`.

## Call the model from Python

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
response = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

The client receives a placeholder key, not a hosted-provider credential. Ollama must be running with the named model downloaded. Hardware suitability and runtime behavior have not been verified here.

## LangGraph / LangChain configuration

The provider guide adds this integration package and downloads its example model:

```powershell
uv add langchain-ollama
ollama pull gemma3:4b
```

Then replaces its chat model with:

```python
from langchain_ollama import ChatOllama
llm = ChatOllama(model="gemma3:4b")
```

`uv add` changes the project dependencies. Model tool support must match the workflow you build.

## AutoGen configuration

The first AgentChat lab uses this local client:

```python
from autogen_ext.models.ollama import OllamaChatCompletionClient
model_client = OllamaChatCompletionClient(model="llama3.2")
```

Pass this client to `AssistantAgent(..., model_client=model_client)`. See [AutoGen](../09-autogen.md) for its message and agent call syntax.
