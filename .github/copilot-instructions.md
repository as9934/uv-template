# Copilot Instructions

## About the Developer
You are working for a 28-year-old named Arijit (Ari) D. Sen. Ari is an award-winning data and investigative reporter currently serving as Data Editor at Scientic American. 

## How to Work

1. Think before coding. Don't assume. Don't hide confusion. Surface tradeoffs. Before implementing: 
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask questions.

2. Simplicity First — Use the minimum amount of code that solves the problem. Nothing speculative. No features beyond what was asked. No abstractions for single-use code. No "flexibility" or "configurability" that wasn't requested. No error handling for impossible scenarios. If you write 200 lines and it could be 50, rewrite it. Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

3. Surgical Changes — Touch only what you must. Clean up only your own mess. When editing existing: Don't refactor things that aren't broken. Match existing style, even if you'd do it differently. If you notice unrelated dead code, mention it - don't delete it. When your changes create orphans: Remove anything that YOUR changes made unused. Don't remove pre-existing dead code unless asked. Every changed line should trace directly to the user's request.

4. Please always speak in plain English, like a human would. Avoid all technical jargon. Please keep your outputs extremely precise. Write in ASD-STE100/Simplified Technical English and follow Zinsser's four principles of quality writing: simplicity, brevity, clarity and humanity. 

5. When you are uncertain about facts, current information or technical details, you should use web search to verify and provide accurate information rather than speculating or admitting uncertainty without investigation. When a problem seems to involve a specific API or library, don't assume you know it; Always check the web for the documentation of the relevant features.

## Mandatory Project Conventions

- Keep projects on the Desktop, create them from the Desktop `uv-template` and manage them with `uv`.
- Add dependencies with `uv add` and run commands with `uv run`.
- Put all analysis code in numbered Jupyter notebooks such as `0_collect.ipynb` and `1_clean.ipynb`.
- Use one notebook per discrete task.
- Keep each cell focused on one thing and use very little code per cell.
- Immediately after creating or transforming a top-level pandas DataFrame, call `df.head()` on it at the bottom of the same cell so the result is visible. If the dataframe is 100 rows or less you may simply call `df` to display it.
- Put all imports in the first code cell.
- Do not write notes to yourself, scratch commentary or implementation reminders in notebooks.
- Do not create Python scripts unless a notebook cannot reasonably do the job.
- Use Pandas for tabular analysis.
- Prefer readable method chains using lambdas, `.pipe(...)` and `.assign(...)`.
- Do not overwrite a dataframe with a transformed version of itself. For example, do not write `companies = companies.loc[...]`; give the result a new very short, descriptive name.
- Do not create unnecessary helper functions. Keep simple transformations directly in the method chain. Keep functions only for things that are reused three+ times.
- Use `snake_case` for every variable and column name.
- Use short, descriptive dataframe names tied to their contents, such as `acs` for American Community Survey data. Never use generic names such as `df`.
- Do not define path variables or path constants. Inline paths in the read or write operation that uses them.
- Do not use `Path()` — either write the actual path or an f-string.
- In notebooks, use direct relative paths such as `../data/created/companies.parquet`
- Do not use `os.mkdir()` or check if a path exists; It should exist because the notebooks need to be run in order.
- Read data directly from its source URL whenever pandas or the relevant library supports it. Download a local copy only when the source or workflow requires one.
- Never drop columns when reading in raw data. Don't restrict fields at ingestion with `usecols=`, `columns=`, a `fields`/`f` API parameter, or by hand-picking keys out of a JSON/API response. Read and keep every column or field the raw source provides; only narrow it down later, deliberately, downstream of the raw read.
- Store source-faithful raw inputs in `data/raw/`.
- Store cleaned single-source data in `data/cleaned/`.
- Store newly created, joined or derived datasets in `data/created/`.

## Never Do
- Write .py files, unless absolutely necessary
- Create new directories or check whether a path exists — the project directories already exist and notebooks are run in order
- Give cost/time estimates without counting actual docs/chars/tokens
- Truncate output without asking
- Reference cell numbers (cells aren't numbered in VSCode)
- Use more than 40GB of RAM
- Commit API keys or credentials
- Use serial commas before "and" or "or" in any type of sequence or list. This is a hard rule that applies to all code comments, docstrings and any generated text.
- Put imports anywhere but the first cell of the notebook
- Use `pip install` or `uv pip install` — always use `uv add` to ensure dependencies are tracked in pyproject.toml
- Try to use ollama or docker without making sure it is running first
- Write non-descriptive variable names like i, j, df, etc. Always use clear, descriptive names that convey their purpose and content.
- Use intermediate variables for simple transformations that can be chained together. For example, instead of:
```python
df = pd.read_csv("data.csv")
df = df.dropna().reset_index(drop=True).copy()
df = df.loc[df["value"] > 0].reset_index(drop=True).copy()
```
Use method chaining:
```python
df = (pd
        .read_csv("data.csv")
        .dropna()
        .loc[lambda x: x["value"] > 0]
        .reset_index(drop=True)
        .copy())
```

## Hardware & Budget
- M4 Pro MacBook, 48GB unified memory — never initiate local operations that will exceed available RAM
- Always run small test batches (10-100 items) before full runs

## File Organization
- Raw data goes in `data/raw/`, cleaned single-source data goes in `data/cleaned/` and newly created or joined data goes in `data/created/` (all gitignored)
- Progress files: `data/*_progress.json`
- No scripts/ directory — everything in notebooks

## Skill Disambiguation

When multiple skills could apply, these rules decide which one wins. My instructions always override skill defaults.

- **CSV/TSV/data cleaning → data-journalism, not xlsx.** The xlsx skill only applies when the *deliverable* is a formatted .xlsx file. If I'm analyzing, cleaning or exploring tabular data and the output is a notebook, chart or story, use data-journalism. The xlsx skill triggers only when I explicitly ask for a spreadsheet file as output.
- **Python over JavaScript.** The pptx and docx skills default to JS libraries (pptxgenjs, docx-js) for creating files from scratch. Prefer Python equivalents (python-pptx, python-docx) unless I specifically ask for JS or the Python library can't do what's needed. The pptx skill's Python-based XML editing workflow (unpack → edit → repack) is fine as-is.
- **Pandas over Excel formulas for analysis.** The xlsx skill says "always use Excel formulas instead of Python." Ignore that when I'm doing data analysis — use pandas and openpyxl together. Only use pure Excel formulas when I ask for a self-updating spreadsheet model that someone else will maintain without Python.
- **Font rules are domain-scoped.** The xlsx skill's "use Arial/Times New Roman" applies only to spreadsheet deliverables. The frontend-design skill's "never use Arial" applies only to web UI. Neither overrides the other.
