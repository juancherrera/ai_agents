# Google Gemini commands and example

Sources: [multi-model lab](../../1_foundations/2_lab2.ipynb), [provider and framework guide](../../guides/09_ai_apis_and_ollama.ipynb).

## Configure

```dotenv
GOOGLE_API_KEY=<YOUR_GOOGLE_KEY>
```

For the course's CrewAI configuration, also use `GEMINI_API_KEY` with the same key. The example below reads `GOOGLE_API_KEY`.

## Call the model

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI(
    api_key=os.environ["GOOGLE_API_KEY"],
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
)
response = client.chat.completions.create(
    model="gemini-2.5-flash",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

This uses Google's compatible endpoint through the OpenAI client. It calls Gemini at Google. For the Agents SDK, the course wraps `AsyncOpenAI` with `OpenAIChatCompletionsModel`; see the provider guide's framework section.
