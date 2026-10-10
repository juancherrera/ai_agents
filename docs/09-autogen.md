# AutoGen AgentChat and Core

Sources: [AgentChat basics](../5_autogen/1_lab1_autogen_agentchat.ipynb), [teams and MCP](../5_autogen/2_lab2_autogen_agentchat.ipynb), [Core runtime](../5_autogen/3_lab3_autogen_core.ipynb), [distributed runtime](../5_autogen/4_lab4_autogen_distributed.ipynb), [world](../5_autogen/world.py).

## Minimal notebook agent

Requires configured model access. This adapts the first lab and closes the client when finished.

```python
from dotenv import load_dotenv
from autogen_ext.models.openai import OpenAIChatCompletionClient
from autogen_agentchat.agents import AssistantAgent
from autogen_agentchat.messages import TextMessage
from autogen_core import CancellationToken

load_dotenv(override=True)
client = OpenAIChatCompletionClient(model="gpt-4o-mini")
try:
    agent = AssistantAgent("teacher", model_client=client)
    response = await agent.on_messages(
        [TextMessage(content="Explain a Python list.", source="user")],
        CancellationToken(),
    )
    print(response.chat_message.content)
finally:
    await client.close()
```

## Concepts

| Concept | Course pattern |
| --- | --- |
| Model client | `OpenAIChatCompletionClient` or `OllamaChatCompletionClient` |
| Message | `TextMessage`; lab 2 adds `MultiModalMessage` |
| Agent | `AssistantAgent` with a model client and optional tools |
| Tool reflection | `reflect_on_tool_use=True` asks the agent to interpret tool results |
| Team | `RoundRobinGroupChat` rotates agents |
| Termination | `TextMentionTermination` stops when the selected text appears |
| LangChain adapter | `LangChainToolAdapter` makes a LangChain tool usable here |
| MCP adapter | `mcp_server_tools` loads tools from a stdio server |

## Core and distributed runtimes

Core is the message-routing layer. The course creates a dataclass message, subclasses `RoutedAgent`, decorates handlers with `@message_handler`, registers agent types in `SingleThreadedAgentRuntime`, starts the runtime, sends a message to an `AgentId`, and stops when idle.

`AgentId` has a type and key. The distributed teaser separates the gRPC host from worker runtimes. See the original lab before running it, since it starts listening services and its cells must be executed in sequence.
