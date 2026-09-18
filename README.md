# Data Visualization — Environment Setup (GitHub Codespaces + uv)

This README documents how this project's Python environment was set up, so it can be reproduced or explained later.

## Overview

- **Platform:** GitHub Codespaces
- **Package/env manager:** [`uv`](https://github.com/astral-sh/uv)
- **Python version:** 3.11.16 (pinned via `uv`, independent of the Codespace's default Python 3.14)
- **Editor:** VS Code (in-browser, via Codespaces) with the Python + Jupyter extensions, plus Amazon Q Developer for AI assistance

## 1. Install `uv`

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

> Note: on this Codespace, the installer's suggested `source $HOME/.local/bin/env` line failed because that file didn't exist at that path. If `uv` isn't found after installing, restart the terminal, or manually add `~/.local/bin` to `PATH`:
> ```bash
> export PATH="$HOME/.local/bin:$PATH"
> ```

## 2. Pin Python 3.11 and initialize the project

```bash
cd /workspaces/data-visualization
uv python install 3.11
uv init --python 3.11
```

This installs an isolated Python 3.11.16 interpreter (separate from the system Python 3.14) and creates a `pyproject.toml` for the project.

## 3. Add visualization dependencies

```bash
uv add matplotlib seaborn pandas numpy plotly jupyterlab
```

`uv add` automatically:
- Creates a `.venv` virtual environment in the project root
- Installs all packages (and their dependencies) into it
- Records exact versions in `uv.lock`

## 4. Project structure created

```bash
mkdir lesson-1
mkdir data
```

## 5. Verify the environment

```bash
uv run python --version
uv run python -c "import matplotlib, seaborn, pandas, plotly; print('all good')"
```

Expected output:
```
Python 3.11.16
all good
```

## 6. Point VS Code at the right interpreter

1. `Ctrl+Shift+P` → **Python: Select Interpreter**
   *(Requires the Microsoft **Python** extension — install it from the Extensions panel if the command doesn't appear.)*
2. Choose the entry labeled:
   ```
   data-visualization (3.11.x)   ./.venv/bin/python   Workspace
   ```

This makes VS Code (and any `.ipynb` notebooks) use the `uv`-managed environment by default, instead of the system Python.

## 7. Activate the environment in the terminal (optional)

```bash
source /workspaces/data-visualization/.venv/bin/activate
```

Confirm with:
```bash
which python
python --version
```
Should point to `/workspaces/data-visualization/.venv/bin/python`, version `3.11.x`.

## 8. AI coding assistant — Amazon Q Developer

- Sign in via the Amazon Q sidebar icon (Builder ID for free/personal use, or IAM Identity Center for org accounts)
- **Chat panel:** ask questions, get explanations, debug errors
- **Inline suggestions:** appear as gray ghost text while typing — `Tab` to accept
- **Right-click commands:** select code → *Amazon Q: Explain / Fix / Refactor / Optimize / Generate Unit Tests*

## Known gotcha from this session

Running `mkdir explore.ipynb` created a **directory**, not a notebook file. Remove it and create the notebook properly through VS Code instead:
```bash
rmdir explore.ipynb
```
Then: `Ctrl+Shift+P` → **Create: New Jupyter Notebook**

## Day-to-day commands cheat sheet

| Task | Command |
|---|---|
| Run a script in the env | `uv run python script.py` |
| Add a new package | `uv add package_name` |
| Remove a package | `uv remove package_name` |
| Launch Jupyter Lab | `uv run jupyter lab --no-browser` |
| Sync env to lockfile | `uv sync` |
| Check interpreter in use | `which python` |
