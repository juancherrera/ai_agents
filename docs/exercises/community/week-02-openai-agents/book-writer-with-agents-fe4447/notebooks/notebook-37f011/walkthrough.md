# notebook

Source: [2_openai/community_contributions/Book Writer with Agents/notebook.ipynb](../../../../../../../2_openai/community_contributions/Book%20Writer%20with%20Agents/notebook.ipynb). Practice: [commented notebook](practice.ipynb).

Cell positions below count all cells, including Markdown. They are not execution counts. The map was extracted from local source cells, never their saved outputs.

## Follow the flow

Configuration stays outside executable examples → Provider endpoint and model selection → Messages → Client, request, and response are separate objects → An agent loop must make progress and stop → Reducer vs. checkpoints vs. conversation identity → Extracted documents become prompt context → Generated code is an output before it is execution

**Before source execution:** This community variant is mapped from its local source. Feature labels describe visible code vocabulary, not a verified execution trace. Read its README/entry point before running the original; external dependencies, models, and side effects are unverified.

Study one concept per sitting. Predict a value, run the corresponding practice cell, explain it, then change one input. If state becomes confusing, restart the practice kernel and run from the top.

## Concepts to understand

### Configuration stays outside executable examples

Environment-variable names identify settings. Values belong in .environment/.env and are loaded deliberately. This exercise inspects only the path and simulated presence flags; it reads no private file or variable values.

**Ask yourself:** Does finding a file prove that authentication works?

**Practice:** Explain the difference between a missing file, a missing variable, and a provider rejecting authentication. Do not print token prefixes or values.

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

### An agent loop must make progress and stop

A loop repeats model/tool steps until a final answer or a configured limit. This fixed script of decisions shows the order without executing generated code or calling a model.

**Ask yourself:** Which decision ends the loop?

**Practice:** Add another tool step before the answer. Keep a finite turn limit and explain how the stopping rule changes.

### Reducer vs. checkpoints vs. conversation identity

A reducer combines state updates. A checkpointer retains state across invocations. A thread_id selects a conversation. This uses SQLite in memory, so it creates no database file.

**Ask yourself:** Does a different thread_id share the same list?

**Practice:** Replace :memory: with a deliberately chosen practice database only if you want persistence. Explain when it would survive process exit.

### Extracted documents become prompt context

The source reads document text and places it in a prompt. This is context injection, not permanent training. A missing page extract must not break concatenation.

**Ask yourself:** Does a page with None add text?

**Practice:** Add a fictional skill and rebuild the prompt. Explain why the existing string does not update itself.

### Generated code is an output before it is execution

A coding workflow designs, writes, and reviews code. Generated files or Docker execution are side effects. First inspect and validate a harmless code string locally.

**Ask yourself:** Does parsing Python text execute it?

**Practice:** Write a test plan for the intended function. Explain which separate step would run code and where the source uses Docker.

## Follow the source in order

### Cell 1: code

- Make available: `os`, `sys`, `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `PIP_MIN`. Re-executing this cell updates kernel state.
- Store expression result in `OFFLINE`. Re-executing this cell updates kernel state.
- Store expression result in `NO_PIP_UPGRADE`. Re-executing this cell updates kernel state.
- Define `_ver_tuple(v)`. Its body runs when called.
- Define `_run(cmd)`. Its body runs when called.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `is_conda`. Re-executing this cell updates kernel state.
- Store expression result in `pip_ver`. Re-executing this cell updates kernel state.
- Store expression result in `should_upgrade`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `parts.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 2: code

- Make available: `os`, `sys`, `platform`, `subprocess`, `shutil`, `importlib.util`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `OFFLINE`. Re-executing this cell updates kernel state.
- Store expression result in `PY`. Re-executing this cell updates kernel state.
- Define `_has_module(name)`. Its body runs when called.
- Define `_pip_install()`. Its body runs when called.
- Define `_run_ok(cmd)`. Its body runs when called.
- Store expression result in `pandoc_path`. Re-executing this cell updates kernel state.
- Store expression result in `HAS_PANDOC`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `HAS_DOCX2PDF`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `soffice_path`. Re-executing this cell updates kernel state.
- Store expression result in `HAS_SOFFICE`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `os.environ.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 3: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `verify_author_calls()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sections.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 4: code

- Make available: `os`, `sys`, `json`, `re`, `hashlib`, `shutil`, `zipfile`, `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `ROOT`. Re-executing this cell updates kernel state.
- Define `now_utc_iso()`. Its body runs when called.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Define `_env_bool(name, default)`. Its body runs when called.
- Define `_mask(s, keep)`. Its body runs when called.
- Store expression result in `OFFLINE`. Re-executing this cell updates kernel state.
- Store expression result in `AUTO_INSTALL_OPENAI`. Re-executing this cell updates kernel state.
- Store expression result in `AUTO_INSTALL_DOTENV`. Re-executing this cell updates kernel state.
- Define `_ensure_dotenv_loaded()`. Its body runs when called.
- Store expression result in `PY_HASH_SEED`. Re-executing this cell updates kernel state.
- Define `stable_json_dumps(obj)`. Its body runs when called.
- Define `sha1(data)`. Its body runs when called.
- Store expression result in `client`. Re-executing this cell updates kernel state.
- Store expression result in `timeout_s`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store dictionary in `REQUEST_OPTS`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `json.dumps`: Convert Python data to JSON text.
- `load_dotenv`: Load configuration into the process; use the explicit repository .environment/.env path in your practice copy.
- `OpenAI`: Construct a compatible model client. Construction is separate from a request.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 5: code

- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store list in `DIRS`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Define `_env_bool(k, default)`. Its body runs when called.
- Define `_env_int(k, default)`. Its body runs when called.
- Define `_env_float(k, default)`. Its body runs when called.
- Define `_env_str(k, default)`. Its body runs when called.
- Define `_env_csv(k, default)`. Its body runs when called.
- Store dictionary in `FICTION_GENRE_PACKS`. Re-executing this cell updates kernel state.
- Store dictionary in `NONFICTION_STRUCTURE_PACKS`. Re-executing this cell updates kernel state.
- Store expression result in `STYLE_PACKS`. Re-executing this cell updates kernel state.
- Store expression result in `CITATION_STYLES`. Re-executing this cell updates kernel state.
- Store expression result in `BOOK_KIND`. Re-executing this cell updates kernel state.
- Store expression result in `GENRE_PACK`. Re-executing this cell updates kernel state.
- Store expression result in `STRUCTURE_PACK`. Re-executing this cell updates kernel state.
- Store expression result in `STYLE_PACK`. Re-executing this cell updates kernel state.
- Store expression result in `CITATION_STYLE`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store list in `default_characters`. Re-executing this cell updates kernel state.
- Store list in `default_outline_seed_thriller`. Re-executing this cell updates kernel state.
- Store dictionary in `book_spec`. Re-executing this cell updates kernel state.
- Store dictionary in `pipeline_config`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store dictionary in `paths`. Re-executing this cell updates kernel state.
- Store dictionary in `CONFIG`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `book_spec.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 6: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `hashlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `dataclasses`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `now_utc_iso()`. Its body runs when called.
- Define `_safe_label(s, max_len)`. Its body runs when called.
- Define `_safe_int(v, default)`. Its body runs when called.
- Define `_safe_float(v, default)`. Its body runs when called.
- Define `stable_json_dumps(obj)`. Its body runs when called.
- Define `sha1(s)`. Its body runs when called.
- Define `read_text(p)`. Its body runs when called.
- Define `write_text(p, s)`. Its body runs when called.
- Define `read_json(p)`. Its body runs when called.
- Define `write_json(p, d, pretty)`. Its body runs when called.
- Define `append_jsonl(p, record)`. Its body runs when called.
- Define `has_file(p)`. Its body runs when called.
- Define `stamp(p, msg)`. Its body runs when called.
- Define `log_exc(label, e)`. Its body runs when called.
- Define `sanitize_md(t, ensure_final_newline)`. Its body runs when called.
- Define `enforce_heading_policy(md, chapter_heading_fmt)`. Its body runs when called.
- Store expression result in `CHARS_PER_TOKEN`. Re-executing this cell updates kernel state.
- Define `count_words(t)`. Its body runs when called.
- Define `approx_tokens(t)`. Its body runs when called.
- Define `make_cache_key(prefix, payload)`. Its body runs when called.
- Define class `CostCapExceededException` using `Exception`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CostEvent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `CostTracker`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `ChapterEnvelope`. Separate the class definition, constructing an instance, and calling its methods.
- Define `log_agent_call(payload, path)`. Its body runs when called.
- Define `update_run_manifest(delta, path)`. Its body runs when called.
- Define `compute_chapter_word_targets(book_spec, cfg, sections_count, ch_num)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `json.dumps`: Convert Python data to JSON text.
- `p.write_text`: Write a text file at the selected path.
- `write_text`: Write a text file at the selected path.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `p.open`: Open a file or resource. Inspect the mode and path before running it.
- `f.write`: Write data to a file-like object; inspect the path and mode.
- `self.events.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `book_spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `cfg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tmpl.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `self._pending.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `self.parent._pending.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 7: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`, `re`, `json`, `time`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BOOK`. Re-executing this cell updates kernel state.
- Store expression result in `PL`. Re-executing this cell updates kernel state.
- Store expression result in `GATES`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS`. Re-executing this cell updates kernel state.
- Define `_fmt_gate_state(d)`. Its body runs when called.
- Define `pmrouter_sys_prompt()`. Its body runs when called.
- Define `author_sys_prompt()`. Its body runs when called.
- Define `editor_sys_prompt()`. Its body runs when called.
- Define `research_sys_prompt()`. Its body runs when called.
- Define `_extract_text_from_msg(msg)`. Its body runs when called.
- Define `_extract_text_from_response(r)`. Its body runs when called.
- Define `chat(model, sysm, userm, temp, max_t, force_json)`. Its body runs when called.
- Define `_bible_or_kb_hash()`. Its body runs when called.
- Define `_prefix_for(stage)`. Its body runs when called.
- Define class `Agent`. Separate the class definition, constructing an instance, and calling its methods.
- Define class `PMRouter`. Separate the class definition, constructing an instance, and calling its methods.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `BOOK.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `client.responses.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `PL.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `client.chat.completions.create`: Create an object or request, depending on the receiver. For chat.completions.create, this is the provider call.
- `default.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `cpath.write_text`: Write a text file at the selected path.
- `PL.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Agent`: Define an agent configuration: responsibilities, model, and available tools.
- `json.dumps`: Convert Python data to JSON text.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `p0.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `pv_map.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `cpath.unlink`: Delete a filesystem path. Do not run it just to inspect a source example.
- `usage.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `v.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 8: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BOOK`. Re-executing this cell updates kernel state.
- Store expression result in `PL`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS`. Re-executing this cell updates kernel state.
- Store expression result in `STYLE_DIR`. Re-executing this cell updates kernel state.
- Define `_parse_terminology(items)`. Its body runs when called.
- Define `_mk_voice_cards(spec)`. Its body runs when called.
- Define `_citation_posture(spec)`. Its body runs when called.
- Define `gen_style(spec)`. Its body runs when called.
- Store expression result in `STYLE_OUTPUTS`. Re-executing this cell updates kernel state.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `sg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `write_text`: Write a text file at the selected path.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `parsed.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `ch.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `packs.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 9: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BOOK`. Re-executing this cell updates kernel state.
- Store expression result in `PL`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS`. Re-executing this cell updates kernel state.
- Define `_safe_int(x, default)`. Its body runs when called.
- Define `_looks_like_ipynb(obj)`. Its body runs when called.
- Define `_json_repair(s)`. Its body runs when called.
- Define `parse_json_loose(raw)`. Its body runs when called.
- Define `_hash_list(xs)`. Its body runs when called.
- Define `_chapter_template(spec)`. Its body runs when called.
- Define `_outline_constraints(spec)`. Its body runs when called.
- Define `_seed_titles(spec)`. Its body runs when called.
- Define `brief(spec)`. Its body runs when called.
- Define `_normalize_fiction_outline(d, n, spec)`. Its body runs when called.
- Define `_normalize_nonfiction_outline(d, n, spec)`. Its body runs when called.
- Define `_fallback_outline(spec)`. Its body runs when called.
- Define `gen_outline(spec, tracker, router)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `PL.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `PL.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `router.author.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `write_text`: Write a text file at the selected path.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `candidates.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `tmpl.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `fixed.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `base.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `ch.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `beats.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `R.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `R2.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `b.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 10: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BOOK`. Re-executing this cell updates kernel state.
- Store expression result in `PL`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS`. Re-executing this cell updates kernel state.
- Define `_ensure_dir(p)`. Its body runs when called.
- Define `_safe_json(path)`. Its body runs when called.
- Define `_default_ref_paths(kind)`. Its body runs when called.
- Define `_load_ref(kind)`. Its body runs when called.
- Define `_write_ref(kind, d)`. Its body runs when called.
- Define `_parse_bullets(md)`. Its body runs when called.
- Define `_merge_fiction(bible, ch, notes_md)`. Its body runs when called.
- Define `_merge_nonfiction(kb, ch, notes_md, claims)`. Its body runs when called.
- Define `merge_continuity_for_chapter(ch, kind)`. Its body runs when called.
- Define `merge_all_continuity()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.dumps`: Convert Python data to JSON text.
- `bible.setdefault(...).append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `kb.setdefault(...).append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `c.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `claims.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `PATHS.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `BOOK.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `kb.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `outline.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `merged.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `bible.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `t.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 11: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Store expression result in `BOOK`. Re-executing this cell updates kernel state.
- Store expression result in `PL`. Re-executing this cell updates kernel state.
- Store expression result in `PATHS`. Re-executing this cell updates kernel state.
- Define `_ensure_dir(p)`. Its body runs when called.
- Define `_safe_int(x, default)`. Its body runs when called.
- Define `_safe_float(x, default)`. Its body runs when called.
- Define `_bool(x, default)`. Its body runs when called.
- Define `ch_dir(ch, sub)`. Its body runs when called.
- Define `outline_ch(pj, ch)`. Its body runs when called.
- Define `parse_editor_blocks(t)`. Its body runs when called.
- Define `end_matter_spec(spec)`. Its body runs when called.
- Store expression result in `EDITOR_AGENT_PREAMBLE`. Re-executing this cell updates kernel state.
- Define `build_editor_instruction(dtext, spec, oc, ch, cfg, tw)`. Its body runs when called.
- Define `sanitize_chapter_markdown(md, spec)`. Its body runs when called.
- Define `build_author_tighten_prompt(chapter_md, ch, oc)`. Its body runs when called.
- Define `_gate_value_shift(router, ch, md_final, envelope)`. Its body runs when called.
- Define `_gate_hook(router, ch, md_final, envelope)`. Its body runs when called.
- Define `_gate_claim_coverage(claims, resolved)`. Its body runs when called.
- Define `process_chapter(ch, cfg, tracker, router, pj)`. Its body runs when called.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `tmpl.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `sg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `re.compile`: Build an executable graph or compiled object; this is separate from running it.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `PL.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `meta.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `MODEL_CAPS.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `write_text`: Write a text file at the selected path.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `labels.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `cfg.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `router.editor.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `json.dumps`: Convert Python data to JSON text.
- `BOOK.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `x.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `oc.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `hits.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `found_blocks.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `router.author.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `accum.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `router.research.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `it.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `body_lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `rebuilt.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `label_to_heading.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `by_marker.setdefault(...).append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `last_R.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `bs.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `by_label.setdefault(...).append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `found_blocks.setdefault(...).extend`: Add every item from an iterable to a list.
- `em_out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `R.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Rt.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `PL.get(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `r.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `cl.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 12: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `re`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_coalesce_spec(spec)`. Its body runs when called.
- Define `_count_words_local(s)`. Its body runs when called.
- Define `_count_unresolved_markers(text)`. Its body runs when called.
- Define `_gather_references_nonfiction(spec)`. Its body runs when called.
- Define `_append_references_md(md, refs)`. Its body runs when called.
- Define `assemble_book(spec)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `lines.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `write_text`: Write a text file at the selected path.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `dedup.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `r.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `chs.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `toc.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `kb.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `it.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `issued.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `c.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `a.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 13: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `re`, `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_count_words_local(s)`. Its body runs when called.
- Define `_count_unresolved_markers(text)`. Its body runs when called.
- Define `_strip_codeblocks(s)`. Its body runs when called.
- Define `_safe_float(x, default)`. Its body runs when called.
- Define `_avg(vals)`. Its body runs when called.
- Define `run_qa(spec)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `enumerate`: Produce indices with values. Check whether the starting index is zero or one.
- `per_ch_gates.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `out.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `bad_headings.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `g.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `vs_vals.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `hk_vals.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `cc_vals.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `long_quote_issues.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `missing_em.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `item.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `missing_list.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `globals(...).get`: Retrieve a value or HTTP resource, depending on the receiver.
- `label.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `missing.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 14: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `re`, `shutil`, `subprocess`, `zipfile`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `typing`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `minimal_docx(p, note)`. Its body runs when called.
- Define `_pandoc_path()`. Its body runs when called.
- Define `_pandoc_version(pandoc)`. Its body runs when called.
- Define `_run(cmd)`. Its body runs when called.
- Define `_coalesce_spec(spec)`. Its body runs when called.
- Define `_gather_refs_for_export(spec)`. Its body runs when called.
- Define `_detect_pdf_engine()`. Its body runs when called.
- Define `export_deliverables(spec)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `flags.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `dedup.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `write_text`: Write a text file at the selected path.
- `.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `meta_yaml.write_text`: Write a text file at the selected path.
- `style_map.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `it.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `data.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `refs.extend`: Add every item from an iterable to a list.
- `refs.append`: Add one item to an existing list; rerunning the cell repeats that addition.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 15: code

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_scan_files(root_dir)`. Its body runs when called.
- Define `_tail_text(path, n)`. Its body runs when called.
- Define `manifest(tr, outcome)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `qa.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `items.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `stats.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `export_log.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `qa_metrics.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `qa_checks.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 16: code

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `make_router()`. Its body runs when called.
- Define `run_all()`. Its body runs when called.
- Define `rerun_chapter(n)`. Its body runs when called.
- Define `resume()`. Its body runs when called.
- Define `verify_author_calls()`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `os.getenv`: Read a named environment setting; check presence rather than printing its value.
- `pipeline_config.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `qa.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `write_text`: Write a text file at the selected path.
- `book_spec.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `p.unlink`: Delete a filesystem path. Do not run it just to inspect a source example.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 17: code

- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `os`, `sys`, `shutil`, `subprocess`, `traceback`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- Store expression result in `docx_path`. Re-executing this cell updates kernel state.
- Store expression result in `pdf_path`. Re-executing this cell updates kernel state.
- Define `_have_exe(cmd)`. Its body runs when called.
- Define `convert_docx_to_pdf(src, dst)`. Its body runs when called.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `m`. Re-executing this cell updates kernel state.
- Define `tree(d)`. Its body runs when called.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `pipeline_config.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `m.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Path(...).write_text`: Write a text file at the selected path.
- `json.dumps`: Convert Python data to JSON text.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

### Cell 18: code

- Make available: `__future__`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `pathlib`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `datetime`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Make available: `json`, `os`, `re`, `shutil`, `subprocess`. Imports do not by themselves perform the later model request, although imported modules can have initialization side effects.
- Define `_safe_json(path)`. Its body runs when called.
- Define `_safe_text(path, limit)`. Its body runs when called.
- Define `_tree(d)`. Its body runs when called.
- Define `_exists_size(p)`. Its body runs when called.
- Define `_sum_dir_bytes(d)`. Its body runs when called.
- Define `_which(cmd)`. Its body runs when called.
- Define `_pandoc_version()`. Its body runs when called.
- Define `_pdf_engine_detect()`. Its body runs when called.
- Define `_now_iso()`. Its body runs when called.
- Define `_outline_chapter_count()`. Its body runs when called.
- Define `_missing_chapters(n)`. Its body runs when called.
- Store expression result in `man`. Re-executing this cell updates kernel state.
- Store expression result in `cost`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `stats`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `qa`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `exports`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `logs_json`. Re-executing this cell updates kernel state.
- Store expression result in `agent_calls_tail`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `last_err`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `run_manifest`. Re-executing this cell updates kernel state.
- Choose a branch using a condition. Predict both the true and false paths before running the source.
- Store expression result in `b_exists`, `b_size`. Re-executing this cell updates kernel state.
- Store expression result in `d_exists`, `d_size`. Re-executing this cell updates kernel state.
- Store expression result in `e_exists`, `e_size`. Re-executing this cell updates kernel state.
- Store expression result in `p_exists`, `p_size`. Re-executing this cell updates kernel state.
- Store expression result in `toc_exists`, `_`. Re-executing this cell updates kernel state.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Visit each item in a loop. Track what is added or changed on each iteration; an async loop awaits incoming items.
- Define `_diagnose()`. Its body runs when called.
- Define `triage_snapshot()`. Its body runs when called.
- Store expression result in `snap`. Re-executing this cell updates kernel state.
- Separate normal work from exception handling. Read the error type to distinguish candidate causes.
- `print`: Show diagnostic output; this does not replace returning a callback result.
- `man.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `qa.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `exports.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `Path(...).write_text`: Write a text file at the selected path.
- `json.loads`: Decode JSON text to Python data; invalid JSON raises an exception.
- `subprocess.run`: Run the receiver workflow. Runner.run can make multiple model/tool calls; server.run can start a listener.
- `d.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `cost.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `stats.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `gm.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `advice.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `manifest.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `json.dumps`: Convert Python data to JSON text.
- `s.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `missing.append`: Add one item to an existing list; rerunning the cell repeats that addition.
- `outline.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `build_s.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `qa_rpt.get`: Retrieve a value or HTTP resource, depending on the receiver.
- `export.get`: Retrieve a value or HTTP resource, depending on the receiver.

**Pause:** which values does this cell require from earlier cells, and what value or side effect does it produce? Run the offline adaptation before the source.

## Definitions to find

| Cell | Name | Kind |
| --- | --- | --- |
| 1 | `_ver_tuple` | function |
| 1 | `_run` | function |
| 2 | `_has_module` | function |
| 2 | `_pip_install` | function |
| 2 | `_run_ok` | function |
| 3 | `verify_author_calls` | function |
| 4 | `now_utc_iso` | function |
| 4 | `_env_bool` | function |
| 4 | `_mask` | function |
| 4 | `_ensure_dotenv_loaded` | function |
| 4 | `stable_json_dumps` | function |
| 4 | `sha1` | function |
| 5 | `_env_bool` | function |
| 5 | `_env_int` | function |
| 5 | `_env_float` | function |
| 5 | `_env_str` | function |
| 5 | `_env_csv` | function |
| 6 | `now_utc_iso` | function |
| 6 | `_safe_label` | function |
| 6 | `_safe_int` | function |
| 6 | `_safe_float` | function |
| 6 | `stable_json_dumps` | function |
| 6 | `sha1` | function |
| 6 | `read_text` | function |
| 6 | `write_text` | function |
| 6 | `read_json` | function |
| 6 | `write_json` | function |
| 6 | `append_jsonl` | function |
| 6 | `has_file` | function |
| 6 | `stamp` | function |
| 6 | `log_exc` | function |
| 6 | `sanitize_md` | function |
| 6 | `enforce_heading_policy` | function |
| 6 | `count_words` | function |
| 6 | `approx_tokens` | function |
| 6 | `make_cache_key` | function |
| 6 | `CostCapExceededException` | class |
| 6 | `CostEvent` | class |
| 6 | `CostTracker` | class |
| 6 | `ChapterEnvelope` | class |
| 6 | `log_agent_call` | function |
| 6 | `update_run_manifest` | function |
| 6 | `compute_chapter_word_targets` | function |
| 6 | `__init__` | function |
| 6 | `_price_record` | function |
| 6 | `estimate_cost` | function |
| 6 | `_can` | function |
| 6 | `reserve` | function |
| 6 | `reconcile` | function |
| 6 | `child_envelope` | function |
| 6 | `summary` | function |
| 6 | `to_dict` | function |
| 6 | `save` | function |
| 6 | `__init__` | function |
| 6 | `_can` | function |
| 6 | `reserve` | function |
| 6 | `reconcile` | function |
| 7 | `_fmt_gate_state` | function |
| 7 | `pmrouter_sys_prompt` | function |
| 7 | `author_sys_prompt` | function |
| 7 | `editor_sys_prompt` | function |
| 7 | `research_sys_prompt` | function |
| 7 | `_extract_text_from_msg` | function |
| 7 | `_extract_text_from_response` | function |
| 7 | `chat` | function |
| 7 | `_bible_or_kb_hash` | function |
| 7 | `_prefix_for` | function |
| 7 | `Agent` | class |
| 7 | `PMRouter` | class |
| 7 | `__init__` | function |
| 7 | `sysm` | function |
| 7 | `run` | function |
| 7 | `__init__` | function |
| 7 | `pick_model` | function |
| 7 | `log` | function |
| 8 | `_parse_terminology` | function |
| 8 | `_mk_voice_cards` | function |
| 8 | `_citation_posture` | function |
| 8 | `gen_style` | function |
| 9 | `_safe_int` | function |
| 9 | `_looks_like_ipynb` | function |
| 9 | `_json_repair` | function |
| 9 | `parse_json_loose` | function |
| 9 | `_hash_list` | function |
| 9 | `_chapter_template` | function |
| 9 | `_outline_constraints` | function |
| 9 | `_seed_titles` | function |
| 9 | `brief` | function |
| 9 | `_normalize_fiction_outline` | function |
| 9 | `_normalize_nonfiction_outline` | function |
| 9 | `_fallback_outline` | function |
| 9 | `gen_outline` | function |
| 9 | `_slice_outer` | function |
| 10 | `_ensure_dir` | function |
| 10 | `_safe_json` | function |
| 10 | `_default_ref_paths` | function |
| 10 | `_load_ref` | function |
| 10 | `_write_ref` | function |
| 10 | `_parse_bullets` | function |
| 10 | `_merge_fiction` | function |
| 10 | `_merge_nonfiction` | function |
| 10 | `merge_continuity_for_chapter` | function |
| 10 | `merge_all_continuity` | function |
| 10 | `norm` | function |
| 11 | `_ensure_dir` | function |
| 11 | `_safe_int` | function |
| 11 | `_safe_float` | function |
| 11 | `_bool` | function |
| 11 | `ch_dir` | function |
| 11 | `outline_ch` | function |
| 11 | `parse_editor_blocks` | function |
| 11 | `end_matter_spec` | function |
| 11 | `build_editor_instruction` | function |
| 11 | `sanitize_chapter_markdown` | function |
| 11 | `build_author_tighten_prompt` | function |
| 11 | `_gate_value_shift` | function |
| 11 | `_gate_hook` | function |
| 11 | `_gate_claim_coverage` | function |
| 11 | `process_chapter` | function |
| 11 | `ex` | function |
| 11 | `_em_items` | function |
| 11 | `collect_heading_blocks` | function |
| 11 | `collect_label_variants` | function |
| 11 | `_tok_est` | function |
| 11 | `_has_support` | function |
| 12 | `_coalesce_spec` | function |
| 12 | `_count_words_local` | function |
| 12 | `_count_unresolved_markers` | function |
| 12 | `_gather_references_nonfiction` | function |
| 12 | `_append_references_md` | function |
| 12 | `assemble_book` | function |
| 13 | `_count_words_local` | function |
| 13 | `_count_unresolved_markers` | function |
| 13 | `_strip_codeblocks` | function |
| 13 | `_safe_float` | function |
| 13 | `_avg` | function |
| 13 | `run_qa` | function |
| 14 | `minimal_docx` | function |
| 14 | `_pandoc_path` | function |
| 14 | `_pandoc_version` | function |
| 14 | `_run` | function |
| 14 | `_coalesce_spec` | function |
| 14 | `_gather_refs_for_export` | function |
| 14 | `_detect_pdf_engine` | function |
| 14 | `export_deliverables` | function |
| 15 | `_scan_files` | function |
| 15 | `_tail_text` | function |
| 15 | `manifest` | function |
| 16 | `make_router` | function |
| 16 | `run_all` | function |
| 16 | `rerun_chapter` | function |
| 16 | `resume` | function |
| 16 | `verify_author_calls` | function |
| 17 | `_have_exe` | function |
| 17 | `convert_docx_to_pdf` | function |
| 17 | `tree` | function |
| 18 | `_safe_json` | function |
| 18 | `_safe_text` | function |
| 18 | `_tree` | function |
| 18 | `_exists_size` | function |
| 18 | `_sum_dir_bytes` | function |
| 18 | `_which` | function |
| 18 | `_pandoc_version` | function |
| 18 | `_pdf_engine_detect` | function |
| 18 | `_now_iso` | function |
| 18 | `_outline_chapter_count` | function |
| 18 | `_missing_chapters` | function |
| 18 | `_diagnose` | function |
| 18 | `triage_snapshot` | function |

## Finish check

Explain the inputs, state changes, and stopping rule in your own words. Identify where model calls and external actions begin. A passing practice fixture does not prove the original hosted workflow works.

[Explicit .environment/.env loading and troubleshooting](../../../../../configuration.md)
