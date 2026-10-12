# 05 · Understand notebook execution

A notebook combines explanations with executable Python. The kernel is the running Python process holding your variables. What matters is the order you execute code, which can differ from the order shown on the page.

## Watch a variable change

Run this complete example:

```python
fruit = "apple"
print(fruit)
fruit = "pear"  # Assignment replaces the value attached to this name.
print(fruit)
print(2 + 2)
```

Output is `apple`, `pear`, then `4`, on separate lines. A notebook also displays the final expression automatically, but `print` makes the result visible in both a notebook and a script.

Now try this notebook experiment: put `fruit = "apple"` in one runnable block and `print(fruit)` in the next. Execute both, then execute `fruit = "pear"` elsewhere. Running the print block again shows `pear`. The kernel remembers the most recent assignment; it does not reread the earlier assignment automatically.

## Make hidden state visible

```python
# A complete example defines its own inputs before using them.
fruits = ["apple", "pear"]
for fruit in fruits:
    print(fruit.upper())
```

Output: `APPLE`, then `PEAR`. `for` repeats its indented body for each item. `.upper()` returns a new uppercase string.

Restart the kernel and run your notebook from top to bottom. If a name now raises `NameError`, its definition was missing or executed out of order. Saved output is only a previous result; it does not recreate variables after a restart.

## Python, Markdown, and terminal commands have different jobs

Python blocks execute code. Markdown blocks display text, headings, and formatted code. In a Python notebook, a leading `!` runs a terminal command, for example `!git status --short`. That syntax is specific to IPython; it is not valid in a normal `.py` file.

To inspect files in portable Python, use this instead:

```python
from pathlib import Path

folder = Path(r"C:\src\github_pub\ai_agents\guides")
print(folder.name)
print(folder.is_dir())
```

In this repository, output is `guides`, then `True`.

## Display formatted text and bounded progress

```python
from IPython.display import Markdown, display
from tqdm.auto import tqdm

display(Markdown("**Practice complete**"))
for number in tqdm(range(3), desc="Practice"):
    result = number * 2
print(result)
```

In a notebook, the first line appears bold and the loop shows a progress indicator. The last printed value is `4`: `range(3)` produces `0`, `1`, and `2`. A bounded loop completes on its own; interrupt a long-running example with the stop button.

## Practice

Change `range(3)` to `range(5)`. Predict the final value before running: `8`. Restart the kernel and run the complete example again; the result should be the same.

---

Source: [05_notebooks.ipynb](../../../../guides/05_notebooks.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
