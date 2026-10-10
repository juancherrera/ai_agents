# OpenAI Agents SDK

Sources: [first agent](../2_openai/1_lab1.ipynb), [tools and handoffs](../2_openai/2_lab2.ipynb), [models and guardrails](../2_openai/3_lab3.ipynb), [research workflow](../2_openai/4_lab4.ipynb).

## Minimal notebook agent with a harmless tool

Requires configured model access. This is a simplified adaptation of the first two labs.

```python
from dotenv import load_dotenv
from agents import Agent, Runner, function_tool

load_dotenv(override=True)

@function_tool
def add(a: int, b: int) -> int:
    """Add two integers."""
    return a + b

agent = Agent(
    name="Math helper",
    instructions="Use the add tool for addition and explain the result briefly.",
    model="gpt-4o-mini",
    tools=[add],
)
result = await Runner.run(agent, "What is 12 plus 30?")
print(result.final_output)
```

`Agent` holds instructions, model, and tools. `Runner.run` executes the interaction loop. `@function_tool` builds a tool definition from the Python function signature and docstring. Use the async script wrapper in [the async reference](04-async-python.md) outside notebooks.

## Patterns to return to

| Pattern | Course usage |
| --- | --- |
| `with trace("name"):` | Group agent activity for inspection; configured tracing can transmit run data |
| `Runner.run_streamed` | Receive events while the response is generated |
| `asyncio.gather(Runner.run(...), ...)` | Run independent agents concurrently |
| `agent.as_tool(...)` | Call another agent and return its result to the caller |
| `handoffs=[other_agent]` | Allow control to transfer to another agent |
| `output_type=YourPydanticModel` | Request structured output |
| `@input_guardrail` | Evaluate input and return a guardrail result |
| `OpenAIChatCompletionsModel` | Wrap an OpenAI-compatible client / model |
| `WebSearchTool()` | Attach the hosted search tool demonstrated in lab 4 |

The research project separates planning, searching, writing, and email delivery. Reuse planning and writing first; connecting the email tool enables actual external delivery. Guardrails and structured outputs require their own evaluation rather than establishing correctness by themselves.
