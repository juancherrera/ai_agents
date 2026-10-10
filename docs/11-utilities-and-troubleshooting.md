# Utilities and troubleshooting

Sources: [PDF and chat](../1_foundations/3_lab3.ipynb), [foundations app](../1_foundations/app.py), [debugging guide](../guides/08_debugging.ipynb), [troubleshooting notebook](../setup/troubleshooting.ipynb), [diagnostics](../setup/diagnostics.py), [Node setup](../setup/SETUP-node.md).

## Gradio chat

The foundations app demonstrates a message history callback. This simplified script is an echo UI and does not call a model:

```python
import gradio as gr

def chat(message, history):
    return f"You said: {message}"

if __name__ == "__main__":
    gr.ChatInterface(chat, type="messages").launch()
```

Run your saved script with `uv run python your_script.py` and open the URL printed by Gradio. The original course replaces the echo with model calls and includes conversation history. Keep private history out of public logs and shared examples.

## PDF text extraction

```python
from pypdf import PdfReader

reader = PdfReader("my_document.pdf")
text = "\n".join(page.extract_text() or "" for page in reader.pages)
print("Extracted characters:", len(text))
```

The file path is relative to the current working directory. This reads text from a PDF; image-only scans may need OCR. The course uses profile text as model context. Choose what personal content to send before connecting a model.

## Debug from evidence

| Problem | First check |
| --- | --- |
| Import error | Run `uv sync`; check `sys.executable` in the notebook |
| `.env` not found | Check the saved filename and root location; inspect only whether a variable is present |
| Authentication error | Check variable name, provider, and endpoint; do not print the key |
| API limit or quota error | Read the provider's error response and account limits |
| Old variables / functions in notebook | Restart the kernel and rerun cells from the top |
| Missing browser executable | Follow the Playwright install commands in setup |
| Node server hangs | Compare Node/npx paths in the environment that launches the server |
| Windows notebook browser `NotImplementedError` | Read the lab 3 note and try the course's Python module version |
| CrewAI Unicode error | See the `PYTHONUTF8` workaround in [commands](02-commands.md) |

The Node guide offers an absolute `npx` path or a subprocess `env` with an adjusted `PATH` for WSL. Verify the actual path before using it. In a notebook, use `os.environ` to update the Python process; a shell `export` in a separate subprocess does not update the kernel.

Read a diagnostic script before running it. Course diagnostics can inspect the machine or make network requests; this documentation build did not execute those scripts.
