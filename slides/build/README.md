# How `agentic_ai_intro.pptx` was generated

`build_pptx.py` + `deck_lib.py` generate the PowerPoint deck from scratch using
[python-pptx](https://python-pptx.readthedocs.io/).

## ⚠️ Read before running

**Re-running the build OVERWRITES `../agentic_ai_intro.pptx` and destroys any edits
you made by hand in PowerPoint.**

Once you start tweaking the deck in PowerPoint, treat the `.pptx` as the source of
truth and these scripts as historical record. Only re-run if you want to start over.

## Running it

```bash
pip install python-pptx
python build_pptx.py
```

## What it produces

Everything is a **native, editable PowerPoint object** — nothing is a flattened image
except one equation:

| Element | How it is built | Editable? |
|---|---|---|
| Body text, titles, bullets | text boxes with real `buChar` bullets | yes |
| Code listings | text boxes in Consolas, per-token syntax colour | yes |
| Tables | native PowerPoint tables | yes |
| Diagram boxes | rounded rectangles | yes |
| Diagram arrows | connectors **glued** to shapes (`begin_connect`/`end_connect`) | yes — arrows follow when you drag a box |
| Slide numbers | a live `slidenum` field | yes — auto-renumbers when you reorder |
| Gradient-jump indicator (slide 43) | `eq/indicator.png`, rendered by LaTeX + dvipng at 900 dpi | no — replace with a PowerPoint equation if you need to edit it |

`eq/indicator.tex` is the source for that one image. Everything else that looks like
maths is Unicode text you can edit directly.

## Inline markup used in the source

`build_pptx.py` writes slide text with a small markup, parsed by `add_runs()`:

- `**bold**`
- `*italic*`
- `` `code` `` — switches to Consolas
- `!!accent!!` — bold, in the deck's dark red

and in bullet lists: `- ` for a top-level bullet, `  - ` for a sub-bullet, `""` for a gap.
