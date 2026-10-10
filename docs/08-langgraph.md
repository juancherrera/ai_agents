# LangGraph

Sources: [graph fundamentals](../4_langgraph/1_lab1.ipynb), [tools and memory](../4_langgraph/2_lab2.ipynb), [async browser tools](../4_langgraph/3_lab3.ipynb), [evaluator workflow](../4_langgraph/4_lab4.ipynb), [Sidekick](../4_langgraph/sidekick.py).

## Small graph with no model call

This adapted example requires LangGraph but no API key.

```python
from typing import TypedDict
from langgraph.graph import StateGraph, START, END

class State(TypedDict):
    text: str

def uppercase(state: State):
    return {"text": state["text"].upper()}

builder = StateGraph(State)
builder.add_node("uppercase", uppercase)
builder.add_edge(START, "uppercase")
builder.add_edge("uppercase", END)
graph = builder.compile()
print(graph.invoke({"text": "hello"}))
```

Intended result: `{'text': 'HELLO'}`. The first course lab demonstrates that graphs can orchestrate ordinary Python functions without an LLM.

## Five construction steps

1. Define state: values shared between nodes.
2. Add nodes: functions that return state updates.
3. Add edges: paths between nodes.
4. Add conditional routing where needed.
5. Compile and invoke the graph.

## Chat, tools, and memory

The course defines chat state with `Annotated[list, add_messages]`. The reducer combines message updates instead of replacing the entire list. The LLM node binds tool definitions; `ToolNode` executes requested calls; `tools_condition` routes to tools or the end of the turn.

Memory requires a checkpointer and conversation identity. The lab uses:

```python
config = {"configurable": {"thread_id": "practice-1"}}
```

Pass the same configuration across turns to continue that conversation; use a different ID for a separate conversation. The course compares `MemorySaver` with SQLite-backed `SqliteSaver`. In-memory checkpoints disappear when the process ends; SQLite stores checkpoints in a database.

For async nodes use `await graph.ainvoke(...)`. The Sidekick adds evaluator feedback, success criteria, and browser tools. Follow the [browser setup](01-environment-setup.md) before its browser exercises. Windows notebook browser limitations and the move to Python modules are discussed in lab 3.
