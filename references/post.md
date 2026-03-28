# `ghst post`

Post management in Ghost.

## Usage
```bash
ghst post [options] [command]
```

## Commands
- `list [options]`: List posts.
- `get [options] [id]`: Get a post by id or slug.
- `create [options]`: Create a post.
- `update [options] [id]`: Update a post by id or slug.
- `delete [options] [id]`: Delete a post.
- `publish [options] <id>`: Publish a post.
- `schedule [options] <id>`: Schedule a post.
- `unschedule <id>`: Unschedule a post.
- `copy <id>`: Copy a post.
- `bulk [options]`: Run bulk post operations.

## Key Options (Common to most list/get commands)
- `--json`: Output as raw JSON.
- `--jq <query>`: Post-process JSON with a jq expression.
- `--limit <count>`: Limit the number of results.
- `--filter <query>`: Apply Ghost API filters.
- `--include <models>`: Include related models (e.g., `tags,authors`).
- `--fields <names>`: Specify which fields to return.
