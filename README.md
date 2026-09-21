 # Expense Tracker Remote MCP Server

An expense-tracking [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server built with FastMCP. It exposes expense management tools and an expense-category resource over HTTP, so an MCP client can connect to it remotely.

## Features

- Add expenses with date, amount, category, subcategory, and note
- List expenses within an inclusive date range
- Summarize expenses by category
- Read supported categories from `categories.json`
- Store data locally in SQLite

## MCP Interface

### Tools

| Tool | Description |
| --- | --- |
| `add_expense` | Add a new expense entry |
| `list_expenses` | List expenses between two dates |
| `summarize` | Calculate totals by category, optionally filtered by category |

### Resource

| Resource | Description |
| --- | --- |
| `expense://categories` | Returns the supported categories and subcategories as JSON |

## Requirements

- Python 3.12 or newer
- `uv` or `pip`

## Run Locally

Using `uv`:

```bash
uv sync
uv run python main.py
```

Using `pip`:

```bash
pip install .
python main.py
```

The server listens on `0.0.0.0` and uses port `8000` by default. Set `PORT` to use another port:

```bash
PORT=8080 python main.py
```

The MCP HTTP endpoint is:

```text
http://localhost:8000/mcp
```

On Windows PowerShell:

```powershell
$env:PORT = "8080"
python main.py
```

## Remote Deployment

1. Push this repository to GitHub.
2. Create a Python service on your hosting provider.
3. Install dependencies with `pip install .` or `uv sync`.
4. Start the service with `python main.py` or `uv run python main.py`.
5. Use the public URL with the `/mcp` path as the remote MCP server URL.
6. Configure the platform-provided `PORT` environment variable if required.

Do not use `/` as the health check endpoint unless your platform accepts a `404` response. The MCP endpoint is `/mcp`.

## Data Storage

Expenses are stored in `expenses.db` beside `main.py`. On hosts with ephemeral disks, data can be lost when the service restarts or redeploys. Use persistent storage before using this server for important data.

## Project Structure

```text
.
├── categories.json   # Expense categories
├── main.py           # FastMCP server and SQLite operations
├── pyproject.toml    # Project metadata and dependencies
├── uv.lock           # Locked dependencies for uv
└── expenses.db       # Created automatically at runtime
```
