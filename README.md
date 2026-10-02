# 2025 Annual Report on Drug-Related Mortality

Quarto **book** project scaffolded from the [Orange County CHA](../Orange%20County%20CHA) pipeline: same HTML/PDF book layout, Excel workbook → figure/table generator, and accessibility styling. Fill in the chapter skeletons, replace the starter workbook, and render.

## What's in here

| Path | Purpose |
|---|---|
| `_quarto.yml` | Book config: title, chapter list, HTML/PDF formats, pre/post-render hooks. |
| `index.qmd` | Landing page (purpose + contacts). |
| `chapters/*.qmd` | Chapter skeletons with `TODO` placeholders and `<!-- OBJECT: ... -->` markers. |
| `chapters/_generated/objects/` | Auto-generated table/figure includes (from the workbook). |
| `scripts/` | Python/Node build pipeline + authoring guides. |
| `includes/` | Accessibility skip-links, Google Translate, PDF LaTeX packages. |
| `templates/` | Word authoring instructions (`TEMPLATE_INSTRUCTIONS.md`). |
| `theme.scss`, `custom-citation-styles.css` | OCDOH styling. |
| `data/raw/workbook.xlsx` | Starter workbook (demo figure + table). Replace with your mortality data. |
| `chapters/99-example-data-objects.qmd` | Live demo of the data pipeline — delete when ready. |

## Quick start

1. Install Python dependencies:
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```
2. Render the book:
   ```bash
   ./render
   ```
   Use `./render` (not bare `quarto render`) when the repo is on Work Drive / an external volume — macOS `._*` sidecar files break Quarto there. Same as `bash scripts/render_html_local_workdir.sh`.
3. Open `docs/index.html` in a browser.

## How the data pipeline works

```
data/raw/workbook.xlsx  ──generate──>  chapters/_generated/objects/<id>.qmd  ──include──>  rendered figure/table
```

1. Workbook schema: **`Master`** roster + **`Dropdowns`** + one sheet per indicator. See `scripts/WORKBOOK_SCHEMA.md`.
2. `./generate-objects` (also a Quarto pre-render hook) writes include files for every active figure/table.
3. Chapters pull objects in with `{{< include _generated/objects/<object_id>.qmd >}}`.

Narrative lives in the `.qmd` chapters (or Word → `scripts/docx_to_qmd.py`). The workbook only drives figures and tables.

> After adding a **new** indicator ID, run `./generate-objects` once before the first render that references it (Quarto resolves includes before pre-render on a first reference).

## Building out the report

1. Edit `_quarto.yml` title/subtitle/author and `index.qmd` contacts if needed.
2. Replace `data/raw/workbook.xlsx` with your mortality workbook (same schema).
3. Fill `TODO`s in `chapters/*.qmd`; swap `<!-- OBJECT: ... -->` markers for real includes after generating objects.
4. Add BibTeX entries to `references.bib`.
5. Delete `chapters/99-example-data-objects.qmd` and its line in `_quarto.yml` when finished with the demo.

## Publishing to GitHub Pages

1. Render with `./render`.
2. Commit `docs/` (including `docs/.nojekyll`) and push to `main`.
3. Pages source: **GitHub Actions** (`.github/workflows/deploy-pages.yml`).

## Accessibility

See `ACCESSIBILITY_CHANGES.md` for skip-links, alt text, and heading conventions.
