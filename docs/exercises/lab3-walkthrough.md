# Lab 3, explained one step at a time

Source: [`1_foundations/3_lab3.ipynb`](../../1_foundations/3_lab3.ipynb), inspected in its current local form. Cell numbers below count **every cell**, including Markdown and the blank cell. They are positions, not execution counts such as `[7]`; edits can shift them.

Use this guide with [the practice notebook](lab3-practice.ipynb). You do not need to understand agent frameworks for this lab. The main building blocks are strings, lists, dictionaries, functions, and an `if` statement.

## What you are building

The first version is a chat assistant that answers using a person's supplied profile. The second adds a quality check:

```text
Profile PDF + summary → system prompt
System prompt + old conversation + new question → draft reply
Profile + conversation + draft → evaluator decision
Accept → return draft
Reject → send feedback to writer → return one revised reply
```

The model does not permanently learn the profile here. Your program includes the profile text in requests. This lab is not fine-tuning, vector search, or retrieval from a database. An evaluator is another model call, not proof that an answer is correct.

Your local notebook has an extra email-recording section after the instructor's main workflow. It is a useful next exercise, but its function and schema are not yet wired into the chat's model calls.

## How to study each step

1. **Predict:** say what the next cell should produce.
2. **Run:** use Shift+Enter for just that cell.
3. **Explain:** describe the result without reading the comment.
4. **Change:** make the suggested small modification, then run it again.

If you get lost, restart the practice notebook kernel and run from the top. Notebook definitions live in memory; skipping cells or running them out of order can leave old values and old functions active.

## 1. Imports and clients — instructor cells 3–4

`from ... import ...` makes an existing class or function available. `import gradio as gr` gives a module a shorter name.

| Name | Job in this lab |
| --- | --- |
| `load_dotenv` | Loads configuration into process environment variables |
| `OpenAI` | Creates a client that can send compatible API requests |
| `PdfReader` | Reads pages and embedded text from a PDF |
| `gradio` / `gr` | Builds a chat interface around your function |

`openai = OpenAI()` stores a client object in a variable named `openai`. It does not generate an answer. The network request happens later at `.create(...)`.

**Your configuration:** use the root's `.environment/.env`, not a bare `load_dotenv()` call. The optional practice cells locate the repository and load that exact path. Tokens stay out of Python source; `.env` itself is still a plaintext file. Never print its values.

**Practice:** inspect `sys.executable`, then explain the difference between importing `OpenAI`, constructing a client, and requesting a completion.

## 2. Read the profile — instructor cells 5–8

`PdfReader("me/linkedin.pdf")` uses a path relative to the **kernel's working directory**, not automatically to the notebook file. From the repository root it would need `1_foundations/me/linkedin.pdf`. The practice notebook resolves paths explicitly.

The loop starts with an empty string and adds text from each page:

```python
linkedin = ""                     # No accumulated text yet.
for page in reader.pages:          # Visit every PDF page.
    text = page.extract_text()     # Can return text, an empty string, or None.
    if text:                      # Add only a nonempty value.
        linkedin += text          # += means keep the old text and append this text.
```

Adding a newline between pages can make boundaries clearer. Text extraction reads embedded text; scanned images may need OCR.

`with open(..., encoding="utf-8") as f` opens a text file and closes it when the block ends. `f.read()` returns its contents as one string. `name` is just another string, not a special model setting.

**Practice:** use the fictional summary and page text first. Predict what happens when one page returns `None`. Then run optional PDF extraction and print only page and character counts. Your profile may contain personal details.

## 3. Build the system prompt — instructor cells 9–10

An f-string substitutes variables: `f"You represent {name}"` includes the current value of `name`.

The prompt has three parts:

1. Role and behavior: represent this person, answer professionally, admit missing information.
2. Evidence: summary and extracted profile.
3. A final reminder about the role.

`system_prompt += ...` appends another string. `\n` inserts a newline; `\n\n` leaves a blank line. The instructor uses a backslash at the end of physical lines to continue a Python expression. Parenthesized strings are another readable way to do this.

**Important notebook behavior:** changing `name` later does not update an already-created prompt. Rerun the prompt-building cell. The same is true after changing the summary or PDF text.

**Practice:** change the fictional name and rebuild the prompt. Check that both the identity and context changed. Explain why the model should say it does not know whether the person has a patent when the supplied profile never mentions one.

## 4. Messages and history — instructor cell 11

The request contains a **list of dictionaries**:

```python
messages = (
    [{"role": "system", "content": system_prompt}]  # Instructions and profile.
    + history                                    # Earlier user/assistant turns.
    + [{"role": "user", "content": message}]     # This question, once.
)
```

`+` joins these lists. Each item describes who spoke and what they said. `history` should contain earlier turns, not another copy of the new message. On a first turn it is normally `[]`.

**Practice:** build a first request and then a follow-up request. Print the roles and count the messages. Remove history temporarily and explain what context the follow-up loses.

## 5. The callback and response — instructor cells 11–13

`def chat(message, history):` defines a function. Gradio later calls it with a new message and previous turns. Defining the function alone does not call the API.

`client.chat.completions.create(...)` sends a model request. In `response.choices[0].message.content`:

- `choices` is a list of returned choices.
- `[0]` selects its first element; Python indexing starts at zero.
- `.message.content` accesses the answer text.

The callback returns that text, and Gradio displays it. `return` sends a value back to the caller; `print` writes diagnostic output.

Cell 12 strips extra history fields for stricter providers:

```python
history = [{"role": h["role"], "content": h["content"]} for h in history]
```

Each `h` is one old message. This keeps only role and content. It matches this lab's text-message exercise; multimodal histories need their own handling.

`gr.ChatInterface(chat, type="messages").launch()` connects the callback to a UI and starts a server. It does not create the callback's intelligence.

**Practice:** run a fake callback before a hosted one. Enter a follow-up question. Enable the optional UI only when ready; the default practice run does not start a server.

## 6. Define a structured decision — instructor cells 14–17

The instructor changes from 'produce an answer' to 'judge an answer'. Pydantic defines the expected decision shape:

```python
class Evaluation(BaseModel):
    is_acceptable: bool  # True or False: should we accept this reply?
    feedback: str       # A reason or an instruction for revision.
```

`Evaluation(is_acceptable=False, feedback="Unsupported patent claim")` is a structured object. Access its fields with `evaluation.is_acceptable` and `evaluation.feedback`.

The evaluator gets its own instructions and the same profile evidence. `evaluator_user_prompt(...)` assembles the old conversation, newest question, and proposed reply. These are distinct inputs: the evaluator needs to know both what was asked and what was answered.

**Practice:** make one accepting decision and one rejecting decision by hand. Then reject the fictional claim 'I hold three patents' because it is not supported by the supplied profile.

## 7. Ask a model to evaluate — instructor cells 18–22

`gemini = OpenAI(..., base_url=...)` uses the compatible client to call Google's endpoint. The variable name alone does not select the provider; the endpoint does.

`gemini.beta.chat.completions.parse(..., response_format=Evaluation)` requests the structured decision and parses it into the Pydantic model. `.message.parsed` retrieves that object. It can be absent on some unsuccessful responses, so the practice example checks it.

The source notebook uses `gemini-2.0-flash` here. Availability and compatible structured-output behavior are **UNVERIFIED at runtime**. The optional exercise lets you specify the evaluator model through configuration instead of silently replacing the instructor's model.

Cells 20–22 generate a patent answer and evaluate it. `messages[:1]` means 'a list containing the first item'; in cell 22 that item is the system message, not a normal chat history. The practice notebook uses `[]` for a first-turn history to make the distinction clear.

**Practice:** use the offline rule-based evaluator first. It is intentionally simple, so you can see the data flow. It is not equivalent to an LLM evaluator and is not a general factuality checker.

## 8. Repair a rejected answer — instructor cell 23

`rerun(...)` builds a new system prompt containing the attempted answer and rejection feedback, then requests another reply to the same question.

The original `system_prompt` remains available. `updated_system_prompt` is a new string; assigning it does not overwrite the original variable.

**Practice:** start with an unsupported claim, give the writer feedback, and produce a revision that admits the missing evidence. Print the draft, decision, and final reply separately.

## 9. Put the workflow together — instructor cells 24–25

The instructor defines `chat` again. The later definition replaces the earlier one in the kernel. Rerun the UI construction cell to attach the function you intend to use.

The new callback does this:

1. Generate a draft.
2. Evaluate it.
3. If accepted, keep it.
4. Otherwise, rerun with feedback.
5. Return the selected reply.

The Pig Latin instruction is deliberate sabotage to exercise the rejection branch when the question contains `patent`. It is a demonstration trigger, not useful profile-assistant behavior. The original check is case-sensitive: `"patent" in message` differs from `"patent" in message.lower()`.

**Important limitation:** the instructor performs **one retry**, then returns the revised answer without evaluating it again. This is not 'retry until correct'. Normal accepted turns use two model calls; rejected turns use three. Retrying increases API usage.

**Practice:** test one accepted question and one deliberately bad patent answer. Explain which branch ran and how many writer/evaluator calls it needed. As an extension, add a second evaluation with a fixed maximum retry count; do not use an unbounded loop.

## 10. Your email/tool extension — local cells 27–29

`record_email_tool(email)` appends an address to `emails.txt` relative to the working directory. Calling it in cell 28 writes immediately. Repeating the cell appends another line.

The schema in cell 29 describes a function name and argument shape. A schema does not execute the function or automatically make it available to the model. The chat call has no `tools=` argument and no tool-result dispatch yet. That is the next lesson's topic.

The final cell currently has `"additionalProperties": false`. Lowercase `false` is JSON syntax. In a Python dictionary, write `False`; otherwise executing the cell raises `NameError` unless someone previously defined a variable named `false`.

The practice notebook uses a **list in memory** instead of saving real addresses. It validates the example schema's shape and calls the function manually, so you can distinguish definition, description, and execution.

## 11. Optional hosted practice and your credentials

Keep all credentials in:

```text
C:\src\github_pub\ai_agents\.environment\.env
```

The optional notebook cells require `OPENAI_API_KEY` for the writer and `GOOGLE_API_KEY` for the evaluator. Optional non-secret settings `LAB3_WRITER_MODEL` and `LAB3_EVALUATOR_MODEL` select models; defaults preserve `gpt-4o-mini` and `gemini-2.0-flash` from this notebook.

Set `RUN_HOSTED = True` only when intentionally making provider calls. The program then loads that explicit file into environment variables. The default is `False` and never reads the file. It never prints keys. Hosted execution sends the fictional profile and question to the configured services and can incur cost.

## If something breaks

| Symptom | Check |
| --- | --- |
| `NameError: summary/system_prompt/...` | Run the defining cell first; restart and run from the top if state is confusing. |
| Missing module | Select the repository `.venv` kernel; run `uv sync` if dependencies are absent. |
| PDF/text file not found | Inspect the resolved path and working directory; use the practice notebook's root finder. |
| PDF text is empty | Check for a scanned/image-only PDF. |
| Authentication fails | Verify the exact `.environment/.env` path and provider variable name without printing values. |
| Model unavailable or parse fails | Read the provider error; check access and structured-output compatibility for the configured model. |
| Chat forgets earlier turns | Check that previous user and assistant messages are included in `history`. |
| UI uses old behavior | Close the earlier UI and rebuild it after redefining the callback. |
| Retry still produces a bad answer | The source does not re-evaluate the retry; inspect it rather than assuming success. |

## Can you explain it now?

Before moving on, answer without looking at code:

1. What is the difference between profile context and conversation history?
2. Why does the request start with a system message?
3. Who calls `chat`, and who displays its return value?
4. What does the Pydantic model guarantee, and what does it not guarantee?
5. What extra information does `rerun` receive?
6. How many retries does the instructor's workflow perform?
7. Does describing an email tool make the model execute it?

Answers: context supplies background evidence; history supplies earlier turns. The system message supplies instructions/context. Gradio calls the callback and displays its return. Pydantic checks structured data, not truth. Rerun receives the rejected draft and feedback. The source retries once. A schema alone does not execute a tool.
