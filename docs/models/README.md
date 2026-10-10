# Commands by model provider

Sources: [multi-model lab](../../1_foundations/2_lab2.ipynb) and [provider guide](../../guides/09_ai_apis_and_ollama.ipynb). These notes extract the course's connection patterns and model identifiers; current service availability is unverified.

| Provider | Configuration variable | Course example model | Reference |
| --- | --- | --- | --- |
| OpenAI | `OPENAI_API_KEY` | `gpt-5-nano` | [OpenAI](openai.md) |
| Anthropic | `ANTHROPIC_API_KEY` | `claude-sonnet-4-5` | [Claude](anthropic.md) |
| Google | `GOOGLE_API_KEY` | `gemini-2.5-flash` | [Gemini](gemini.md) |
| DeepSeek | `DEEPSEEK_API_KEY` | `deepseek-chat` | [DeepSeek](deepseek.md) |
| Groq | `GROQ_API_KEY` | `openai/gpt-oss-120b` | [Groq](groq.md) |
| OpenRouter | `OPENROUTER_API_KEY` | `openai/gpt-4o-mini` | [OpenRouter](openrouter.md) |
| Ollama locally | No service key; client placeholder `ollama` | `llama3.2` | [Ollama](ollama.md) |

## Shared setup commands

From the repository root:

```powershell
uv sync
uv run python my_example.py
```

`uv sync` installs this project's dependencies. Save a Python example as `my_example.py` to run it from the terminal, or paste it into a notebook using the project's `.venv` kernel. Hosted examples require a local root `.env` with your own provider key. See [environment setup](../01-environment-setup.md).

The `openai` Python client is used for several providers because the course uses their OpenAI-compatible endpoints. The `base_url` selects the service; `model` selects the model at that service. Anthropic's course example instead uses its own client and response shape.

## Categories to remember

| Task | Command / Python operation |
| --- | --- |
| Load local configuration | `load_dotenv(override=True)` |
| Read a key | `os.environ["PROVIDER_API_KEY"]` |
| Connect | `OpenAI(api_key=..., base_url=...)` or `Anthropic()` |
| Send a prompt with the compatible API | `client.chat.completions.create(model=..., messages=...)` |
| Read its answer | `response.choices[0].message.content` |
| Send an Anthropic prompt | `client.messages.create(..., max_tokens=...)` |
| Read its answer | `response.content[0].text` |
| Download a local model | `ollama pull llama3.2` |
| List downloaded local models | `ollama ls` |

Each provider page includes a self-contained example. Hosted calls are unverified at runtime and may incur costs.
