# Anthropic / Claude commands and example

Source: [week-one multi-model lab](../../1_foundations/2_lab2.ipynb).

## Configure

```dotenv
ANTHROPIC_API_KEY=<YOUR_ANTHROPIC_KEY>
```

## Call the model

```python
from dotenv import load_dotenv
from anthropic import Anthropic

load_dotenv(override=True)
client = Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1000,
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.content[0].text)
```

The course uses Anthropic's native client. `max_tokens` is required in this example and bounds the requested output. Its answer is in `content[0].text`, a different response shape from the compatible chat examples. This minimal example requests a plain text answer.
