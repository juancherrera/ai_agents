# OpenAI commands and example

Source: [week-one multi-model lab](../../1_foundations/2_lab2.ipynb).

## Configure

In your local root `.env`:

```dotenv
OPENAI_API_KEY=<YOUR_OPENAI_KEY>
```

## Call the model

```python
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI()
response = client.chat.completions.create(
    model="gpt-5-nano",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

`OpenAI()` reads `OPENAI_API_KEY`. `messages` supplies the conversation; `model` selects the course model. The response is read from the first choice's message.

## Agent framework

The course's SDK examples use `Agent(..., model="gpt-4o-mini")` and `await Runner.run(agent, prompt)`. See [OpenAI Agents SDK](../06-openai-agents-sdk.md) for tools, streaming, handoffs, and structured outputs.
