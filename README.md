# Expense Tracker MCP

A lightweight Model Context Protocol (MCP) server for tracking personal and household expenses with SQLite persistence and category-based summaries.

This project exposes expense-management tools that can be used by MCP clients such as Claude Desktop or other AI tooling to add expenses, list transactions, and summarize spending by category over a date range.

## Features

- Add new expense entries with date, amount, category, optional subcategory, and notes
- List expenses within a selected date range
- Summarize totals by category
- Persistent storage in SQLite
- Predefined category taxonomy via `categories.json`
- Easy integration with MCP-compatible clients

## Project structure

```text
.
├── main.py                 # MCP server and expense tools
├── categories.json         # Built-in category/subcategory taxonomy
├── expenses.db             # SQLite database for persisted expenses
├── pyproject.toml          # Python project metadata and dependencies
├── .python-version         # Python version pin
├── uv.lock                 # Locked dependency graph
├── src/
│   └── expense_tracker_mcp/
│       └── __init__.py     # Package stub / entry point placeholder
├── README.md               # Project documentation
└── .gitignore
```

## Available tools

The server currently exposes these MCP tools:

### `add_expense`
Add a new expense record.

Parameters:
- `date`: expense date (format should be consistent with your app, e.g. `YYYY-MM-DD`)
- `amount`: numeric amount
- `category`: expense category (for example `food`, `transport`, `housing`)
- `subcategory`: optional subcategory string
- `note`: optional note or description

### `list_expenses`
Returns all expenses between a start and end date.

Parameters:
- `start_date`
- `end_date`

### `summarize`
Returns total expenditure grouped by category.

Parameters:
- `start_date`
- `end_date`
- `category`: optional filter by a single category

### `expense://categories`
A resource exposing the category taxonomy from `categories.json` as JSON.

## Installation

This project uses Python and `uv` for dependency management.

```bash
# Optional: create a virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install uv
uv sync
```

## Running the server

Start the MCP server with:

```bash
uv run python main.py
```

You can then connect this server from any MCP-compatible client.

## Example MCP client configuration

Example configuration for an MCP client that launches the server via `uv`:

```json
{
  "mcpServers": {
    "expense-tracker": {
      "command": "uv",
      "args": ["run", "python", "/absolute/path/to/expense-tracker-mcp/main.py"]
    }
  }
}
```

## Example usage

### Add an expense
```python
add_expense(
    date="2026-11-07",
    amount=120.50,
    category="food",
    subcategory="dining_out",
    note="Dinner with friends"
)
```

### List expenses in a date range
```python
list_expenses(start_date="2026-11-01", end_date="2026-11-30")
```

### Summarize expenses by category
```python
summarize(start_date="2026-11-01", end_date="2026-11-30")
```

## Categories

The default taxonomy is defined in `categories.json`, including categories such as:

- `food`
- `transport`
- `housing`
- `utilities`
- `health`
- `education`
- `shopping`
- `travel`
- `investments`
- `misc`

Each category contains subcategories like `groceries`, `fuel`, `rent`, `internet_broadband`, and more.

## Notes

This repository is intentionally small and focused on serving as a practical example of an MCP-based expense tracker. It uses a local SQLite database and simple JSON-backed category metadata, making it easy to extend for additional features such as:

- monthly budgets
- recurring expenses
- CSV export
- category editor UI
- authentication or multi-user support

## License

This project does not currently declare a license file. If you plan to publish or share it publicly, consider adding an appropriate license (for example MIT or Apache 2.0).
