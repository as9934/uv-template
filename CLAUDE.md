# Project Instructions

## Project setup

- Keep this project on the Desktop and manage it with `uv`.
- Start new projects from the Desktop `uv-template`.
- Add dependencies with `uv add` and run commands with `uv run`.
- Put all analysis code in numbered Jupyter notebooks such as `0_collect.ipynb` and `1_clean.ipynb`.
- Use one notebook per discrete task.
- Keep each cell focused on one thing and use very little code per cell.
- Put all imports in the first code cell.
- Do not write notes to yourself, scratch commentary or implementation reminders in notebooks.
- Do not create Python scripts unless a notebook cannot reasonably do the job.

## Pandas style

- Use pandas for tabular analysis.
- Prefer readable method chains using lambdas, `.pipe(...)` and `.assign(...)`.
- Do not overwrite a dataframe with a transformed version of itself. For example, do not write `companies = companies.loc[...]`; give the result a new short, descriptive name.
- Do not create unnecessary helper functions. Keep simple transformations directly in the method chain.
- Use `snake_case` for every variable and column name.
- Use short, descriptive dataframe names tied to their contents, such as `acs` for American Community Survey data. Never use generic names such as `df`.

## Paths and data

- Do not define path variables or path constants. Inline paths in the read or write operation that uses them.
- Read data directly from its source URL whenever pandas or the relevant library supports it. Download a local copy only when the source or workflow requires one.
- Store source-faithful raw inputs in `data/raw/`.
- Store cleaned single-source data in `data/cleaned/`.
- Store newly created, joined or derived datasets in `data/created/`.
