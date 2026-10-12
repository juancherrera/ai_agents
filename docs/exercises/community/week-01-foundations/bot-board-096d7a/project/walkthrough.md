# bot board: project walkthrough

Practice: [commented offline exercises](practice.ipynb).

This module/project pack is extracted from the local files listed below. Functions and line numbers are source locations; the original program is not executed by practice.

## Read the project in this order

1. README and dependency manifest: intended setup and command.
2. Entry point (`main.py`, `app.py`, or the documented launcher): starting input.
3. Worker/crew/graph definitions: who handles it and what state is passed.
4. Tool/server definitions: where external operations begin.
5. Output/storage and cleanup: what changes and when execution stops.

## Concepts to understand

### Provider endpoint and model selection

A compatible client can target different services. The endpoint selects the provider and its credentials; the model identifier selects a model there. Compatibility does not guarantee every feature works.

**Ask yourself:** Does changing only the variable name change the provider?

**Practice:** Add a provider from the linked source. Copy its endpoint and variable name, never a credential value.

### Messages: instructions, earlier turns, and the new question

The model request is a list of messages. A role says who is speaking; content holds the text. The system instruction and prior conversation serve different purposes.

**Ask yourself:** How many messages are sent on the follow-up turn?

**Practice:** Set history to an empty list. Explain what is lost while the system instruction remains.

### Client, request, and response are separate objects

Constructing a client prepares a connection interface. A request invokes it. The returned response contains choices, a message, and its content. This fixture imitates that nested shape.

**Ask yourself:** What does choices[0] select?

**Practice:** Add a second choice with different text. Read choices[1] and explain why choosing an index matters.

### Evaluate, give feedback, and bound revision

A worker produces a draft and an evaluator judges it against criteria. Feedback becomes input for another attempt. The evaluator here is a small deterministic teaching rule, not a general quality checker.

**Ask yourself:** Is the first draft acceptable? How many attempts will be allowed?

**Practice:** Make the second draft fail too. Add an explicit message when the finite retry limit is reached.

## Module-by-module reading map

### [1_foundations/community_contributions/bot_board/conversation_context.py](../../../../../../1_foundations/community_contributions/bot_board/conversation_context.py)

**Imports:**

- `IPython.display` — Notebook display/shell facilities.
- `collections` — Local or third-party dependency; follow its definition/import before execution.
- `conversation_role` — Local or third-party dependency; follow its definition/import before execution.
- `conversation_state` — Local or third-party dependency; follow its definition/import before execution.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `ConversationContext` | class |
| 15 | `__init__` | function |
| 22 | `reset` | function |
| 27 | `set_conversation_state` | function |
| 34 | `add_callback` | function |
| 45 | `get_context` | function |
| 48 | `get_conversation_state` | function |
| 51 | `get_next_conversation_state` | function |
| 54 | `update_context` | function |
| 59 | `add_response` | function |
| 71 | `print_context` | function |
| 84 | `should_participate` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `collections`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `conversation_state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `conversation_role`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `IPython.display`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ConversationContext`. Separate the class definition, constructing an instance, and calling its methods.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self.context.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self._callbacks.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `display`: Render a value inside a notebook.
- `self.context.extend`: Add every item from an iterable to a list.
- `msg.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/bot_board/conversation_role.py](../../../../../../1_foundations/community_contributions/bot_board/conversation_role.py)

**Imports:**

- `enum` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `ConversationRole` | class |
| 12 | `__str__` | function |

**Read in this order:**

- Make available: `enum`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ConversationRole` using `Enum`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/bot_board/conversation_state.py](../../../../../../1_foundations/community_contributions/bot_board/conversation_state.py)

**Imports:**

- `enum` — Local or third-party dependency; follow its definition/import before execution.

| Source line | Definition | Kind |
| --- | --- | --- |
| 3 | `ConversationState` | class |
| 14 | `__str__` | function |
| 17 | `next_state` | function |

**Read in this order:**

- Make available: `enum`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define class `ConversationState` using `Enum`. Separate the class definition, constructing an instance, and calling its methods.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

### [1_foundations/community_contributions/bot_board/member.py](../../../../../../1_foundations/community_contributions/bot_board/member.py)

**Imports:**

- `conversation_context` — Local or third-party dependency; follow its definition/import before execution.
- `conversation_role` — Local or third-party dependency; follow its definition/import before execution.
- `conversation_state` — Local or third-party dependency; follow its definition/import before execution.
- `openai` — Compatible model clients and request/response types.
- `typing` — Type hints and state schemas.

| Source line | Definition | Kind |
| --- | --- | --- |
| 7 | `generate_user_content` | function |
| 93 | `get_shared_context` | function |
| 100 | `Member` | class |
| 105 | `set_shared_context` | function |
| 112 | `get_shared_context` | function |
| 115 | `__init__` | function |
| 122 | `__generate_response` | function |
| 126 | `__generate_system_content` | function |
| 132 | `__generate_messages` | function |
| 141 | `get_member_response` | function |
| 153 | `set_conversation_role` | function |

**Read in this order:**

- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `openai`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `conversation_context`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `conversation_state`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `conversation_role`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `generate_user_content(prompt)`. Its body runs when called.
- Define `get_shared_context()`. Its body runs when called.
- Define class `Member`. Separate the class definition, constructing an instance, and calling its methods.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `self.client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `messages.extend`: Add every item from an iterable to a list.
- `messages.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Exercise:** choose one defined function. Trace its arguments → local calculations → return value. Mark any network call, file/database write, generated-code execution, or runtime start separately. Recreate the calculation using fictional inputs before connecting its dependencies.

## Configuration boundary

Use [.environment/.env configuration](../../../../configuration.md). The practice code uses no credentials. The source may initialize files/databases during import; source launchers are not import tests. Original provider/transport behavior is UNVERIFIED at runtime.
