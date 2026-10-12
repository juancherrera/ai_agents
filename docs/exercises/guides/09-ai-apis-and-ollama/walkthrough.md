# 09 · Follow a model request from Python to its answer

The `openai` package is a client library: it sends requests and turns responses into Python objects. It does not run a model inside your script. With a compatible provider, `base_url` changes the destination while the calling pattern remains similar. Compatibility and model capabilities still vary by provider.

## Understand the message and answer without any connection

```python
messages = [{"role": "user", "content": "What is 2 + 2?"}]
# A sample response, not a live model answer.
response = {"choices": [{"message": {"content": "4"}}]}
print(messages[0]["content"])
print(response["choices"][0]["message"]["content"])
```

Output: `What is 2 + 2?`, then `4`. `choices` is a list; `[0]` selects its first answer. The client library exposes response fields as object attributes, so the corresponding real expression is `response.choices[0].message.content`.

## A complete hosted request, explicitly opt-in

This script is disabled by default. It reads `.environment/.env` only after you choose to enable it. Add `OPENAI_MODEL` in that private configuration with a model available to your account, as well as `OPENAI_API_KEY`. No credential value belongs in this script.

```python
import os
from pathlib import Path

RUN_HOSTED = False  # Change to True only when you want a real request.

if RUN_HOSTED:
    from dotenv import load_dotenv
    from openai import OpenAI

    env_path = Path(r"C:\src\github_pub\ai_agents\.environment\.env")
    if not env_path.is_file():
        raise FileNotFoundError("Expected .environment/.env in the repository")
    load_dotenv(env_path, override=False)
    # [] raises KeyError immediately if a required variable is missing.
    client = OpenAI(api_key=os.environ["OPENAI_API_KEY"], timeout=30, max_retries=0)
    response = client.chat.completions.create(
        model=os.environ["OPENAI_MODEL"],
        messages=[{"role": "user", "content": "What is 2 + 2?"}],
    )
    print(response.choices[0].message.content)
else:
    print("Hosted request disabled")
```

Default output: `Hosted request disabled`. When enabled, the client authenticates, sends one request, waits for the response, and prints its answer. Exact answer text is model-dependent. A 30-second timeout bounds waiting and `max_retries=0` disables client retries. Live authentication, provider availability, billing, and answer quality were not tested for this lesson.

## Switching provider requires a matched destination, key, and model

The source notebook uses these example configurations; this table records its examples rather than promising current model availability:

| Provider | Base URL in the source | Credential variable | Source model example |
| --- | --- | --- | --- |
| Gemini | `https://generativelanguage.googleapis.com/v1beta/openai/` | `GOOGLE_API_KEY` | `gemini-2.5-flash-preview-05-20` |
| DeepSeek | `https://api.deepseek.com/v1` | `DEEPSEEK_API_KEY` | `deepseek-chat` |
| OpenRouter | `https://openrouter.ai/api/v1` | `OPENROUTER_API_KEY` | `openai/gpt-4.1-nano` |
| Ollama | `http://localhost:11434/v1` | Local placeholder | `llama3.2` |

For a compatible hosted provider, replace the client construction with `OpenAI(base_url=provider_url, api_key=os.environ[key_variable], timeout=30, max_retries=0)` and set its accepted model name. `provider_url` and `key_variable` must be defined for the provider you selected. Never send one provider's key to another provider's URL.

## Local Ollama follows the same request shape

If Ollama is installed, these PowerShell commands download the source guide's example model and show installed models. Downloading needs network access and disk space.

```powershell
ollama pull llama3.2
ollama list
```

Then use this opt-in Python script with Ollama running:

```python
RUN_LOCAL_MODEL = False
if RUN_LOCAL_MODEL:
    from openai import OpenAI
    client = OpenAI(
        base_url="http://localhost:11434/v1",
        api_key="ollama",  # Nonsecret placeholder required by the client.
        timeout=30,
        max_retries=0,
    )
    response = client.chat.completions.create(
        model="llama3.2",
        messages=[{"role": "user", "content": "What is 2 + 2?"}],
    )
    print(response.choices[0].message.content)
else:
    print("Local model request disabled")
```

Default output: `Local model request disabled`. `localhost` means this computer. The model executes through the local service, not through the Python client. Local model execution was not tested here.

## How frameworks wrap the connection

For the Agents SDK, the source connects an async client to an adapter, then gives the adapter to an agent. This disabled example constructs the objects only; it does not run an agent:

```python
BUILD_AGENT = False
if BUILD_AGENT:
    from openai import AsyncOpenAI
    from agents import Agent, OpenAIChatCompletionsModel, set_tracing_disabled

    set_tracing_disabled(True)
    client = AsyncOpenAI(base_url="http://localhost:11434/v1", api_key="ollama")
    model = OpenAIChatCompletionsModel(model="llama3.2", openai_client=client)
    agent = Agent(name="Tutor", instructions="Explain simply.", model=model)
    print(agent.name)
```

`AsyncOpenAI` is the asynchronous connection; `OpenAIChatCompletionsModel` adapts it to the SDK's model interface; `Agent` adds instructions. CrewAI uses its LLM configuration (the source notes `GEMINI_API_KEY` for Gemini). LangGraph can use `ChatOllama`; AutoGen can use `OllamaChatCompletionClient`. Their configuration objects are different even when they ultimately call the same model service. Check the installed framework version and supported features before replacing a client.

## Practice

Add a system message to the offline `messages` list: `{"role": "system", "content": "Answer briefly."}` before the user message. Explain which item `[0]` now selects. It selects the system message because inserting an item changes list positions.

## Instructor code, included here

These are the code examples from the local guide, in their original order, so you can compare the instructor's syntax with the explanations above without opening another file. These excerpts can depend on earlier definitions. The complete teaching examples above include their own inputs.

Source examples that intentionally fail, run indefinitely, download software, or contact a service are labeled below. Hosted configuration reads have been changed to your explicit `.environment/.env` path; credential values are never included.

### Using different API providers with Agent Frameworks

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
# Create an OpenAI python client for making web calls to OpenAI
openai = OpenAI()

# Make the call
response = openai.chat.completions.create(model="gpt-4.1-mini", messages=[{"role":"user", "content": "what is 2+2?"}])

# Print the result
print(response.choices[0].message.content)
```

### Using different API providers with Agent Frameworks

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
ANTHROPIC_BASE_URL = "https://api.anthropic.com/v1/"
DEEPSEEK_BASE_URL = "https://api.deepseek.com/v1"
GEMINI_BASE_URL = "https://generativelanguage.googleapis.com/v1beta/openai/"
GROK_BASE_URL = "https://api.x.ai/v1"
GROQ_BASE_URL = "https://api.groq.com/openai/v1"
OPENROUTER_BASE_URL = "https://openrouter.ai/api/v1"
OLLAMA_BASE_URL = "http://localhost:11434/v1"
```

### Using different API providers with Agent Frameworks

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
import os
from openai import OpenAI
from dotenv import load_dotenv
load_dotenv(r"C:\src\github_pub\ai_agents\.environment\.env", override=False)

GEMINI_BASE_URL = "https://generativelanguage.googleapis.com/v1beta/openai/"
google_api_key = os.getenv("GOOGLE_API_KEY")
gemini = OpenAI(base_url=GEMINI_BASE_URL, api_key=google_api_key)
response = gemini.chat.completions.create(model="gemini-2.5-flash-preview-05-20", messages=[{"role":"user", "content": "what is 2+2?"}])
print(response.choices[0].message.content)
```

### Using different API providers with Agent Frameworks

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
import os
from openai import OpenAI
from dotenv import load_dotenv
load_dotenv(r"C:\src\github_pub\ai_agents\.environment\.env", override=False)

DEEPSEEK_BASE_URL = "https://api.deepseek.com/v1"
deepseek_api_key = os.getenv("DEEPSEEK_API_KEY")
deepseek = OpenAI(base_url=DEEPSEEK_BASE_URL, api_key=deepseek_api_key)
response = deepseek.chat.completions.create(model="deepseek-chat", messages=[{"role":"user", "content": "what is 2+2?"}])
print(response.choices[0].message.content)
```

### Using different API providers with Agent Frameworks

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
!ollama pull llama3.2

from openai import OpenAI

OLLAMA_BASE_URL = "http://localhost:11434/v1"
ollama = OpenAI(base_url=OLLAMA_BASE_URL, api_key="anything")
response = ollama.chat.completions.create(model="llama3.2", messages=[{"role":"user", "content": "what is 2+2?"}])
print(response.choices[0].message.content)
```

### Using different API providers with Agent Frameworks

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
import os
from openai import OpenAI
from dotenv import load_dotenv
load_dotenv(r"C:\src\github_pub\ai_agents\.environment\.env", override=False)

OPENROUTER_BASE_URL = "https://openrouter.ai/api/v1"
openrouter_api_key = os.getenv("OPENROUTER_API_KEY")
openrouter = OpenAI(base_url=OPENROUTER_BASE_URL, api_key=openrouter_api_key)
response = openrouter.chat.completions.create(model="openai/gpt-4.1-nano", messages=[{"role":"user", "content": "what is 2+2?"}])
print(response.choices[0].message.content)
```

### You need to do this one time on your computer

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```shell
ollama pull llama3.2
```

### You need to do this one time on your computer

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
# You need to do this one time on your computer
!ollama pull llama3.2

from openai import OpenAI
MODEL = "llama3.2"
openai = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")

response = openai.chat.completions.create(
 model=MODEL,
 messages=[{"role": "user", "content": "What is 2 + 2?"}]
)

print(response.choices[0].message.content)
```

### OpenRouter: Convenient gateway platform for OpenAI and others

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
MODEL = "gpt-4o-mini"
openai = OpenAI()
```

### OpenRouter: Convenient gateway platform for OpenAI and others

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
MODEL = "openai/gpt-4o-mini"
openrouter_api_key = os.getenv("OPENROUTER_API_KEY")
openai = OpenAI(base_url="https://openrouter.ai/api/v1", api_key=openrouter_api_key)

response = openai.chat.completions.create(
 model=MODEL,
 messages=[{"role": "user", "content": "What is 2 + 2?"}]
)

print(response.choices[0].message.content)
```

### Set the default OpenAI client for the Agents SDK

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
DEEPSEEK_BASE_URL = "https://api.deepseek.com/v1"
deepseek_client = AsyncOpenAI(base_url=DEEPSEEK_BASE_URL, api_key=deepseek_api_key)
deepseek_model = OpenAIChatCompletionsModel(model="deepseek-chat", openai_client=deepseek_client)
```

### Set the default OpenAI client for the Agents SDK

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# extra imports
from agents import OpenAIChatCompletionsModel
from openai import AsyncOpenAI

# Step 1: specify the base URL endpoints where the provider offers an OpenAI compatible API
GEMINI_BASE_URL = "https://generativelanguage.googleapis.com/v1beta/openai/"
GROK_BASE_URL = "https://api.x.ai/v1"
GROQ_BASE_URL = "https://api.groq.com/openai/v1"
OPENROUTER_BASE_URL = "https://openrouter.ai/api/v1"
OLLAMA_BASE_URL = "http://localhost:11434/v1"

# Step 2: Create an AsyncOpenAI object for that endpoint
gemini_client = AsyncOpenAI(base_url=GEMINI_BASE_URL, api_key=google_api_key)
grok_client = AsyncOpenAI(base_url=GROK_BASE_URL, api_key=grok_api_key)
groq_client = AsyncOpenAI(base_url=GROQ_BASE_URL, api_key=groq_api_key)
openrouter_client = AsyncOpenAI(base_url=OPENROUTER_BASE_URL, api_key=openrouter_api_key)
ollama_client = AsyncOpenAI(base_url=OLLAMA_BASE_URL, api_key="ollama")

# Step 3: Create a model object to provide when creating an Agent
gemini_model = OpenAIChatCompletionsModel(model="gemini-2.5-flash", openai_client=gemini_client)
grok_3_model = OpenAIChatCompletionsModel(model="grok-3-mini-beta", openai_client=openrouter_client)
llama3_3_model = OpenAIChatCompletionsModel(model="llama-3.3-70b-versatile", openai_client=groq_client)
grok_3_via_openrouter_model = OpenAIChatCompletionsModel(model="x-ai/grok-3-mini-beta", openai_client=openrouter_client)
llama_3_2_local_model = OpenAIChatCompletionsModel(model="llama3.2", openai_client=ollama_client)
```

### Set the default OpenAI client for the Agents SDK

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
from openai import AsyncAzureOpenAI
from agents import set_default_openai_client
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv(r"C:\src\github_pub\ai_agents\.environment\.env", override=False)

# Create OpenAI client using Azure OpenAI
openai_client = AsyncAzureOpenAI(
    api_key=os.getenv("AZURE_OPENAI_API_KEY"),
    api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
    azure_deployment=os.getenv("AZURE_OPENAI_DEPLOYMENT")
)

# Set the default OpenAI client for the Agents SDK
set_default_openai_client(openai_client)
```

### LangGraph with other models

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
from langchain_ollama import ChatOllama
# llm = ChatOpenAI(model="gpt-4o-mini")
llm = ChatOllama(model="gemma3:4b")
```

### model_client = OpenAIChatCompletionClient(model="gpt-4o-mini")

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# model_client = OpenAIChatCompletionClient(model="gpt-4o-mini")

from autogen_ext.models.ollama import OllamaChatCompletionClient

model_client = OllamaChatCompletionClient(
    model="gemma3:4b",
    model_info={
        "vision": True,
        "function_calling": False,
        "json_output": True,
        "family": "unknown",
    },
)
```

### model_client = OpenAIChatCompletionClient(model="gpt-4o-mini")

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
import os
from autogen_core.models import ModelInfo

g_model_client = OpenAIChatCompletionClient(
    model="gemini-2.5-flash",
    api_key=os.environ["GEMINI_API_KEY"],
    base_url="https://generativelanguage.googleapis.com/v1beta/openai",
    model_info=ModelInfo(
        vision=True,
        function_calling=True,
        json_output=True,
        family="gemini-2.5",
        structured_output=True
    )
)
```

---

Source: [09_ai_apis_and_ollama.ipynb](../../../../guides/09_ai_apis_and_ollama.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
