# Project Instructions
1. Think before coding. Don't assume. Don't hide confusion. Surface tradeoffs. Before implementing: 
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask questions.

2. Simplicity First — Use the minimum amount of code that solves the problem. Nothing speculative. No features beyond what was asked. No abstractions for single-use code. No "flexibility" or "configurability" that wasn't requested. No error handling for impossible scenarios. If you write 200 lines and it could be 50, rewrite it. Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

3. Surgical Changes — Touch only what you must. Clean up only your own mess. When editing existing: Don't refactor things that aren't broken. Match existing style, even if you'd do it differently. If you notice unrelated dead code, mention it - don't delete it. When your changes create orphans: Remove anything that YOUR changes made unused. Don't remove pre-existing dead code unless asked. Every changed line should trace directly to the user's request.

## Project setup

- Keep this project on the Desktop and manage it with `uv`.
- Start new projects from the Desktop `uv-template`.
- Add dependencies with `uv add` and run commands with `uv run`.
- Put all analysis code in numbered Jupyter notebooks such as `0_collect.ipynb` and `1_clean.ipynb`.
- Use one notebook per discrete task.
- Keep each cell focused on one thing and use very little code per cell.
- Immediately after creating or transforming a top-level pandas DataFrame, call `df.head()` a the bottom of the same cell the df was created in so the result is visible. 
- Put all imports in the first code cell.
- Do not write notes to yourself, scratch commentary or implementation reminders in notebooks.
- Do not create Python scripts unless a notebook cannot reasonably do the job. 

## Pandas style

- Use pandas for tabular analysis.
- Prefer readable method chains using lambdas, `.pipe(...)` and `.assign(...)`.
- Do not overwrite a dataframe with a transformed version of itself. For example, do not write `companies = companies.loc[...]`; give the result a new short, descriptive name.
- Do not create unnecessary helper functions. Keep simple transformations directly in the method chain.
- Do not create a function for one-off column cleanup or normalization; write it visibly with pandas string methods inside `.assign(...)`. Keep functions only for genuinely reused or complex operations such as API retries, parsing or checkpoint writes.
- Use `snake_case` for every variable and column name.
- Use short, descriptive dataframe names tied to their contents, such as `acs` for American Community Survey data. Never use generic names such as `df`.

## Paths and data

- Do not define path variables or path constants. Inline paths in the read or write operation that uses them.
- Do not use `Path()` — either write the actual path or an f-string.
- In notebooks, use direct relative paths such as `../data/created/companies.parquet`
- Do not use `os.mkdir()` or check if a path exists. It should exist because the notebooks need to be run in order.
- Read data directly from its source URL whenever pandas or the relevant library supports it. Download a local copy only when the source or workflow requires one.
- Never drop columns when reading in raw data. Don't restrict fields at ingestion with `usecols=`, `columns=`, a `fields`/`f` API parameter, or by hand-picking keys out of a JSON/API response. Read and keep every column or field the raw source provides; only narrow it down later, deliberately, downstream of the raw read.
- Store source-faithful raw inputs in `data/raw/`.
- Store cleaned single-source data in `data/cleaned/`.
- Store newly created, joined or derived datasets in `data/created/`.