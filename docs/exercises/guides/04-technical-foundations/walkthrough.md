# 04 · What connects your code to a model?

An API is an interface another program exposes. For a hosted model, your Python program sends a request to a URL; a server runs the model and sends a response. A client library handles the HTTP details. Your program still supplies the messages, model choice, and credentials.

## Inspect the data before involving a server

```python
import json

request = {
    "model": "example-model",  # A label here, not a real API call.
    "messages": [{"role": "user", "content": "What is 2 + 2?"}],
}
wire_text = json.dumps(request)  # Convert Python data to JSON text.
restored = json.loads(wire_text)  # Convert JSON text back to Python data.
print(restored["messages"][0]["content"])
```

Output: `What is 2 + 2?`. A Python dictionary and JSON are related but different: one is an in-memory object, the other is a text format exchanged between programs. `[0]` selects the first list item because Python indexes start at zero.

## Load credentials only when you choose to make a real request

Save this as a script or run it in a notebook when you want to load your configuration. It reads your private file; the offline exercises do not run this block.

```python
import os
from pathlib import Path
from dotenv import load_dotenv

env_path = Path(r"C:\src\github_pub\ai_agents\.environment\.env")
if not env_path.is_file():
    raise FileNotFoundError(f"Configuration file missing: {env_path}")
load_dotenv(env_path, override=False)
# Check presence without printing the credential or any part of it.
print("OPENAI_API_KEY configured:", bool(os.getenv("OPENAI_API_KEY")))
```

`Path` represents a filesystem path. The `r` prefix keeps backslashes literal in this Windows path. `load_dotenv` reads variable assignments into the process environment; `os.getenv` retrieves one. `override=False` preserves a variable already supplied to the process. Python scripts contain variable names, not the credential values. The `.env` file itself still contains those values and must remain private.

## Separate the layers when something fails

| Symptom | What to investigate |
| --- | --- |
| Import fails | Is the package installed in the interpreter running this code? |
| Configuration missing | Is the exact file present, and is the required variable defined? |
| Name cannot resolve | DNS, hostname spelling, VPN/network configuration |
| Connection times out | Connectivity, firewall, service availability |
| Certificate error | Certificate trust and proxy configuration |
| Server rejects authentication | Correct provider key and endpoint pairing |
| Request rejected | Model identifier and request fields accepted by that provider |

An endpoint is the destination; a client is the object making requests; a model name selects what the destination runs. Changing only the model name does not automatically move the request to another provider.

## Practice

Add a second message to `request["messages"]` in the offline example. Print its length: `2`. Serialize it again and confirm both messages survive the round trip. This is the same data preparation you will do before a real API call.

---

Source: [04_technical_foundations.ipynb](../../../../guides/04_technical_foundations.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
