# Model Context Protocol (MCP)

Sources: [connecting servers](../6_mcp/1_lab1.ipynb), [accounts](../6_mcp/2_lab2.ipynb), [memory and market data](../6_mcp/3_lab3.ipynb), [push server](../6_mcp/push_server.py), [accounts client](../6_mcp/accounts_client.py).

MCP connects a client to servers that expose tools, resources, and prompt templates. A stdio server is a subprocess communicating through standard input/output. The agent framework calls the tools exposed by that server.

## Simple server script

Save as `practice_server.py` at the repository root. This harmless adaptation uses the course's `FastMCP` tool decorator.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("practice")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two integers."""
    return a + b

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

The client starts this script. Running it directly in a terminal makes it wait for protocol input; it is not a chat interface. Keep diagnostic prints off stdout because stdout carries the MCP protocol.

## List tools from a notebook

Run from the repository root after saving the script above. No model is needed just to list tools.

```python
from agents.mcp import MCPServerStdio

params = {"command": "uv", "args": ["run", "practice_server.py"]}
async with MCPServerStdio(
    params=params, client_session_timeout_seconds=30
) as server:
    tools = await server.list_tools()
    print([tool.name for tool in tools])
```

To give an SDK agent access, construct it with `mcp_servers=[server]` and run it **inside** the `async with` block so the connection remains open.

## Course launch patterns

| Server | Command / arguments in the course |
| --- | --- |
| Fetch | `uvx`, `['mcp-server-fetch']` |
| Playwright | `npx`, `['@playwright/mcp@latest']` |
| Filesystem | `npx`, `['-y', '@modelcontextprotocol/server-filesystem', sandbox_path]` |
| Course accounts | `uv`, `['run', 'accounts_server.py']` from `6_mcp/` |
| Course market | `uv`, `['run', 'market_server.py']` from `6_mcp/` |

The Node package names above preserve the course examples; `@latest` resolves a changing version. Server arguments are a list, with one entry per argument. Relative script and database paths depend on the subprocess working directory. The filesystem server receives an allowed directory; choose it deliberately.

The accounts and memory examples write local databases. Do not run reset or trading code just to test connectivity. Begin with `list_tools()` and the harmless practice server.
