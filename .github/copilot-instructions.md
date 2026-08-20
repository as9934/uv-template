# Copilot Instructions

## About the Developer
You are working for a 28-year-old named Arijit (Ari) D. Sen. Ari is an award-winning data and investigative reporter currently serving as Data Editor at Scientic American. 

His language of choice is Python, and he has some experience with JavaScript, SQL and R. He has experience in data analysis, web scraping, NLP and building data pipelines. He is comfortable with command line tools, Jupyter notebooks and using APIs. 

His GitHub is as9934. 

He prefers to work inside Jupyter Notebooks within VSCode. 

For more understanding about data journalism and the way Ari likes his code, use your data journalism skill. If he asks you to do something with web scraping look at the web scraping skill. And if he asks you something to do with filing a public records request (colloquially known as a FOIA request) look at the FOIA skill.

## Mandatory Project Conventions

- Keep projects on the Desktop, create them from the Desktop `uv-template` and manage them with `uv`.
- Add dependencies with `uv add` and run commands with `uv run`.
- Put all analysis code in numbered Jupyter notebooks such as `0_collect.ipynb` and `1_clean.ipynb`.
- Use one notebook per discrete task.
- Keep each cell focused on one thing and use very little code per cell.
- Immediately after creating or transforming a top-level pandas DataFrame, add a separate `dataframe_name.head()` cell so the result is visible. Guard the preview with the same condition when the DataFrame is created conditionally.
- Put all imports in the first code cell.
- Do not write notes to yourself, scratch commentary or implementation reminders in notebooks.
- Use pandas for tabular analysis.
- Prefer readable method chains using lambdas, `.pipe(...)` and `.assign(...)`.
- Do not overwrite a dataframe with a transformed version of itself. For example, do not write `companies = companies.loc[...]`; give the result a new short, descriptive name.
- Do not create unnecessary helper functions. Keep simple transformations directly in the method chain.
- Do not create a function for one-off column cleanup or normalization; write it visibly with pandas string methods inside `.assign(...)`. Keep functions only for genuinely reused or complex operations such as API retries, parsing or checkpoint writes.
- Use `snake_case` for every variable and column name.
- Use short, descriptive dataframe names tied to their contents, such as `acs` for American Community Survey data. Never use generic names such as `df`.
- Do not define path variables or path constants. Inline paths in the read or write operation that uses them.
- In notebooks, use direct relative paths such as `../data/created/companies.parquet`; never calculate a project root with `Path.cwd()`.
- Use `Path` only where a path method is required, for example `Path("../data/created/output").mkdir(parents=True, exist_ok=True)`.
- Read data directly from its source URL whenever pandas or the relevant library supports it. Download a local copy only when the source or workflow requires one.
- Never drop columns when reading in raw data. Don't restrict fields at ingestion with `usecols=`, `columns=`, a `fields`/`f` API parameter, or by hand-picking keys out of a JSON/API response. Read and keep every column or field the raw source provides; only narrow it down later, deliberately, downstream of the raw read.
- Store source-faithful raw inputs in `data/raw/`.
- Store cleaned single-source data in `data/cleaned/`.
- Store newly created, joined or derived datasets in `data/created/`.

## Never Do
- Write .py files, unless absolutely necessary
- Create new directories without checking if one already exists
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
- Say "There it is", "Honestly", "That confirms it" or similar filler phrases when responding to user queries

## Communication Style
- Be direct and concise, no hedging
- Give concrete numbers, not ranges
- When estimating compute: count actual items, measure one, multiply
- When I say "do it" or "yes" — execute immediately, don't explain more

## Hardware & Budget
- M4 Pro MacBook, 48GB unified memory — never initiate local operations that will exceed available RAM
- Modal: ~$30-100 per pipeline run budget. Always use 10x A100 80GB.
- Ollama Cloud: $20/mo subscription, use for query-time LLMs (or when I tell you to)
- Always run small test batches (10-100 items) before full runs

## File Organization
- Benchmarks go in `analysis/benchmarks/`
- Raw data goes in `data/raw/`, cleaned single-source data goes in `data/cleaned/` and newly created or joined data goes in `data/created/` (all gitignored)
- Progress files: `data/*_progress.json`
- No scripts/ directory — everything in notebooks

## Skill Disambiguation

When multiple skills could apply, these rules decide which one wins. My instructions always override skill defaults.

- **CSV/TSV/data cleaning → data-journalism, not xlsx.** The xlsx skill only applies when the *deliverable* is a formatted .xlsx file. If I'm analyzing, cleaning or exploring tabular data and the output is a notebook, chart or story, use data-journalism. The xlsx skill triggers only when I explicitly ask for a spreadsheet file as output.
- **Python over JavaScript.** The pptx and docx skills default to JS libraries (pptxgenjs, docx-js) for creating files from scratch. Prefer Python equivalents (python-pptx, python-docx) unless I specifically ask for JS or the Python library can't do what's needed. The pptx skill's Python-based XML editing workflow (unpack → edit → repack) is fine as-is.
- **Pandas over Excel formulas for analysis.** The xlsx skill says "always use Excel formulas instead of Python." Ignore that when I'm doing data analysis — use pandas and openpyxl together. Only use pure Excel formulas when I ask for a self-updating spreadsheet model that someone else will maintain without Python.
- **Font rules are domain-scoped.** The xlsx skill's "use Arial/Times New Roman" applies only to spreadsheet deliverables. The frontend-design skill's "never use Arial" applies only to web UI. Neither overrides the other.
