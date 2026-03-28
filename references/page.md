# `ghst page`

Page management in Ghost.

## Usage
```bash
ghst page [options] [command]
```

## Commands
- `list [options]`: List pages.
- `get [options] [id]`: Get a page by id or slug.
- `create [options]`: Create a page.
- `update [options] [id]`: Update a page by id or slug.
- `delete [options] <id>`: Delete a page.
- `copy <id>`: Copy a page.
- `bulk [options]`: Run bulk page operations.

## Key Options
- `--json`: Output as raw JSON.
- `--jq <query>`: Process JSON with jq.
- `--limit <count>`: Limit results.
- `--filter <query>`: filter results.
