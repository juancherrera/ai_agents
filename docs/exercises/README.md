# Exercises: learn by slowing down

Start with **[Lab 3 walkthrough](lab3-walkthrough.md)**, then open **[Lab 3 practice notebook](lab3-practice.ipynb)** beside the instructor's [`1_foundations/3_lab3.ipynb`](../../1_foundations/3_lab3.ipynb).

The walkthrough explains the instructor's sequence. The notebook breaks it into small, commented exercises with predictions, expected results, and answer checks. Run one cell at a time with Shift+Enter. Stop after each exercise and explain the result in your own words.

The practice notebook defaults to fictional profile data and simulated model replies. It needs no API key, does not read your private environment file, does not launch a server, and does not save email addresses. Optional hosted calls explicitly load **`.environment/.env`** from the repository root; those calls send the selected profile context to the providers.

Select the repository's `.venv` notebook kernel. If imports fail, run `uv sync` from the repository root, then reselect the kernel. To check the kernel, run `import sys; print(sys.executable)`.

## Suggested sessions

1. Sections 1–4: imports, paths, PDF/text inputs, prompt construction.
2. Sections 5–6: message history, model responses, and the Gradio callback.
3. Sections 7–9: structured evaluation, feedback, and the retry workflow.
4. Sections 10–11: the email/tool extension and optional real model calls.

Repeat a section until you can predict its output before running it. There is no need to complete all sections in one sitting.
