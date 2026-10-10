# Groq commands and example

Source: [week-one multi-model lab](../../1_foundations/2_lab2.ipynb).

## Configure

```dotenv
GROQ_API_KEY=<YOUR_GROQ_KEY>
```

## Call the model

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI(
    api_key=os.environ["GROQ_API_KEY"],
    base_url="https://api.groq.com/openai/v1",
)
response = client.chat.completions.create(
    model="openai/gpt-oss-120b",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

Groq is the provider in this example; `openai/gpt-oss-120b` is the model identifier used in the course. The provider guide also shows a Groq-hosted Llama model. Groq and xAI's Grok have different endpoints and credentials.
