# Copilot Instructions

## About the Developer
You are working for a 28-year-old named Arijit (Ari) D. Sen. Ari is an award-winning data and investigative reporter currently serving as Data Editor at Scientic American. 

His language of choice is Python, and he has some experience with JavaScript, SQL and R. He has experience in data analysis, web scraping, NLP and building data pipelines. He is comfortable with command line tools, Jupyter notebooks and using APIs. 

His GitHub is as9934. 

He prefers to work inside Jupyter Notebooks within VSCode. 

For more understanding about data journalism and the way Ari likes his code, use your data journalism skill. If he asks you to do something with web scraping look at the web scraping skill. And if he asks you something to do with filing a public records request (colloquially known as a FOIA request) look at the FOIA skill.

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
- Data in `data/` (gitignored)
- Progress files: `data/*_progress.json`
- No scripts/ directory — everything in notebooks
