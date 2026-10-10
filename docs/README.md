# My AI engineering reference library

Use this library to find a command, understand a pattern, and return to the course lesson that demonstrates it.

## Start here

| I want to… | Reference |
| --- | --- |
| Pick a provider and find its setup and call syntax | [Model provider index](models/README.md) |
| Set up Python, uv, API keys, or a notebook kernel | [Environment setup](01-environment-setup.md) |
| Look up terminal, Git, uv, CrewAI, or browser commands | [Command cheat sheet](02-commands.md) |
| Refresh Python, JSON, classes, or type hints | [Python patterns](03-python-patterns.md) |
| Understand async, await, gather, and streaming | [Async Python](04-async-python.md) |
| Call a model, maintain messages, or use tools | [Model APIs and tools](05-model-apis-and-tools.md) |
| Build an OpenAI SDK agent | [OpenAI Agents SDK](06-openai-agents-sdk.md) |
| Build a crew with agents and tasks | [CrewAI](07-crewai.md) |
| Build a graph with state, routing, and memory | [LangGraph](08-langgraph.md) |
| Use AutoGen AgentChat or Core | [AutoGen](09-autogen.md) |
| Connect an agent to an MCP server | [MCP](10-mcp.md) |
| Make a UI, read a PDF, or debug a failure | [Utilities and troubleshooting](11-utilities-and-troubleshooting.md) |

## How to use the examples

The short examples are simplified adaptations of the repository's course material. Each page links to its sources. They describe the APIs used by this checkout; they are not claims about the latest package releases, model availability, pricing, or platform requirements. Use the repository's `uv.lock` and the original lessons as your starting point.

Terminal blocks run in a shell. Python blocks run in a notebook cell unless labeled **script**. In notebooks, `await` can be used directly. In scripts, put async work inside `async def main()` and call `asyncio.run(main())`. A leading `!` is notebook shell syntax; remove it when entering a command in a terminal.

Examples using hosted models require your own configured credentials and can incur costs. Only the syntax and local standard-library examples have been verified here; hosted APIs, framework integrations, installations, and deployments are **UNVERIFIED at runtime**. Extracts may contain intentionally failing exercises or functions that send email, modify files, or write databases.

## Organization

`models/` groups commands by provider: OpenAI, Anthropic, Gemini, DeepSeek, Groq, OpenRouter, and local Ollama. The numbered references cover shared environment commands, Python patterns, and each agent framework. These are curated notes with simple examples rather than full notebook copies. Source links point back to the course for the longer exercises.

Search examples from the project root:

```powershell
rg -n "base_url|API_KEY|ollama pull" docs/models
rg -n "uv sync|thread_id|asyncio.gather" docs
```
