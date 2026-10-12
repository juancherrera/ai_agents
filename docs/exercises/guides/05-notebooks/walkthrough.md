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

## Instructor code, included here

These are the code examples from the local guide, in their original order, so you can compare the instructor's syntax with the explanations above without opening another file. These excerpts can depend on earlier definitions. The complete teaching examples above include their own inputs.

Source examples that intentionally fail, run indefinitely, download software, or contact a service are labeled below. Hosted configuration reads have been changed to your explicit `.environment/.env` path; credential values are never included.

### A broader guide to Notebooks with examples

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Click anywhere in this cell and press Shift + Return

2 + 2
```

### Congrats!

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Set a value for a variable

favorite_fruit = "bananas"
```

### Congrats!

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# The result of the last statement is shown after you run it

favorite_fruit
```

### Congrats!

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Use the variable

print(f"My favorite fruit is {favorite_fruit}")
```

### Congrats!

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Now change the variable

favorite_fruit = f"anything but {favorite_fruit}"
```

### Now go back and rerun the cell with the print statement, two cells back

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
# Then run this cell twice, and see if you understand what's going on

print(f"My favorite fruit is {favorite_fruit}")

favorite_fruit = "apples"
```

### Explaining the 'kernel'

Track the value assigned to fruit or fruits before this runs. The kernel keeps previous assignments until restart.

```python
print(f"My favorite fruit is {favorite_fruit}")
```

### Cell output

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
spams = ["spam"] * 1000
print(spams)

# Might be worth clearing output after running this!
```

### And a sub-sub-head

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```text
# This is a heading
## This is a sub-head
### And a sub-sub-head

I like Jupyter Lab because it's
- Easy
- Flexible
- Satisfying
```

### The exclamation point

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
# list the current directory

!ls
```

### The exclamation point

Optional environment/service example: this can load private configuration, contact a service, or run a terminal command. It was not executed in the offline checks.

```python
# ping cnn.com - press the stop / interrupt button in the toolbar when you're bored

!ping cnn.com
```

### Minor things we encounter on the course

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# Here's some code with no progress bar
# It will take 10 seconds while you wonder what's happpening..

import time

spams = ["spam"] * 1000

for spam in spams:
    time.sleep(0.01)
```

### Minor things we encounter on the course

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# And now, with a nice little progress bar:

import time
from tqdm import tqdm

spams = ["spam"] * 1000

for spam in tqdm(spams):
    time.sleep(0.01)
```

### Minor things we encounter on the course

Read the comments and follow each assignment to its next use. Any name not defined here comes from an earlier example in this section.

```python
# On a different topic, here's a useful way to print output in markdown

from IPython.display import Markdown, display

display(Markdown("# This is a big heading!\n\n- And this is a bullet-point\n- So is this\n- Me, too!"))
```

---

Source: [05_notebooks.ipynb](../../../../guides/05_notebooks.ipynb). This lesson expands the locally documented topics with teaching examples; linked external briefings were not used as source contents.

Optional: [practice notebook](practice.ipynb) · [all guide lessons](../README.md).
