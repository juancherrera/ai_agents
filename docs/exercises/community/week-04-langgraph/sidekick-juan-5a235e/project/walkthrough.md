# sidekick juan: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### A guardrail makes a stop/continue decision

An input guardrail checks a request before later work. The tripwire is a boolean. This deterministic example teaches the branch; it is not the model-based name detector in the source.

**Ask yourself:** Does Alice trigger this demonstration guardrail?

**Practice:** Try another name and observe the limitation. Describe how you would evaluate false positives and missed names.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Retrieve evidence before answering

A retrieval workflow selects relevant chunks and passes them as context. This tiny keyword example illustrates the boundary without embeddings, a vector store, or a hosted model.

**Ask yourself:** Which chunk matches the query about Python?

**Practice:** Try a query with no matches. Return an explicit lack-of-evidence result instead of inventing an answer.

## Configuration and supporting files

- [4_langgraph/community_contributions/sidekick_juan/README.md](../../../../../../4_langgraph/community_contributions/sidekick_juan/README.md): SidekickAI. Read local setup/run instructions before invoking the entry point; remote linked material is unverified.

## Reading exercise

This contribution supplies documents/configuration rather than Python modules. Restate its expected inputs and outputs; follow the linked notebook packs for executable examples.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
