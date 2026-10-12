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

---

Source: [09_ai_apis_and_ollama.ipynb](../../../../guides/09_ai_apis_and_ollama.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
