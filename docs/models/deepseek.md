# DeepSeek commands and example

Sources: [multi-model lab](../../1_foundations/2_lab2.ipynb), [provider guide](../../guides/09_ai_apis_and_ollama.ipynb).

## Configure

```dotenv
DEEPSEEK_API_KEY=<YOUR_DEEPSEEK_KEY>
```

## Call the model

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)
client = OpenAI(
    api_key=os.environ["DEEPSEEK_API_KEY"],
    base_url="https://api.deepseek.com/v1",
)
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[{"role": "user", "content": "Explain Python lists in one sentence."}],
)
print(response.choices[0].message.content)
```

## Use with the Agents SDK

After loading configuration, this notebook pattern adapts the provider guide:

```python
import os
from openai import AsyncOpenAI
from agents import Agent, Runner, OpenAIChatCompletionsModel

client = AsyncOpenAI(
    api_key=os.environ["DEEPSEEK_API_KEY"],
    base_url="https://api.deepseek.com/v1",
)
model = OpenAIChatCompletionsModel(model="deepseek-chat", openai_client=client)
agent = Agent(name="Teacher", instructions="Explain Python simply.", model=model)
try:
    result = await Runner.run(agent, "What is a list?")
    print(result.final_output)
finally:
    await client.close()
```

This configures model access; separately check tracing configuration when using a non-OpenAI provider with the SDK.
