# OpenRouter commands and example

Source: [provider guide](../../guides/09_ai_apis_and_ollama.ipynb).

## Configure

```dotenv
OPENROUTER_API_KEY=<YOUR_OPENROUTER_KEY>
```

## Call the model

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI(
    api_key=os.environ["OPENROUTER_API_KEY"],
    base_url="https://openrouter.ai/api/v1",
)
response = client.chat.completions.create(
    model="openai/gpt-4o-mini",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

The guide uses OpenRouter as a gateway to models from several providers. The course model identifier includes its provider prefix. Use an OpenRouter key with this endpoint. The Agents SDK connection follows the `AsyncOpenAI` plus `OpenAIChatCompletionsModel` pattern shown in [DeepSeek](deepseek.md), replacing the key, URL, and model.
