---
name: ghst
description: Work with Ghost blogs using the ghst CLI tool. Supports full Ghost Admin API access including posts, pages, members, tags, newsletters, themes, stats, social web (ActivityPub), and more.
homepage: https://github.com/TryGhost/ghst
metadata: {"clawdbot":{"emoji":"👻","homepage":"https://github.com/TryGhost/ghst","requires":{"bins":["ghst"],"env":["GHOST_URL","GHOST_STAFF_ACCESS_TOKEN"]},"primaryEnv":"GHOST_URL","install":[{"id":"ghst-npm","kind":"node","package":"@tryghost/ghst","bins":["ghst"],"label":"Install ghst (npm)"}]}}
---

# `ghst` CLI Skill

This skill wraps the [ghst](https://github.com/TryGhost/ghst) CLI tool, which allows the agent to interact directly with any Ghost instance via the Ghost Admin API.

## Configuration & Authentication

To interact with a Ghost instance, the agent requires a Ghost API URL and a Ghost Staff Access Token. The bot owner must provide these either via the `~/.openclaw/.env` file or the OpenClaw configuration file (`~/.openclaw/openclaw.json`).

### Instructions for Bot Owners

1. Navigate to your Ghost Admin panel.
2. Go to **Settings** -> **Staff** (or **Users**) -> Edit your User Profile.
3. At the bottom, generate or copy a **Staff Access Token**.

**Option 1: Using `.env` (Recommended)**

Add the variables directly to your `~/.openclaw/.env` file:

```env
GHOST_URL="https://your-blog-url.ghost.io"
GHOST_STAFF_ACCESS_TOKEN="your-staff-access-token-id:secret"
```

**Option 2: Using `openclaw.json`**

Alternatively, add the following to your OpenClaw config under the `skills.entries.ghst.env` block:

```json
{
  "skills": {
    "entries": {
      "ghst": {
        "enabled": true,
        "env": {
          "GHOST_URL": "https://your-blog-url.ghost.io",
          "GHOST_STAFF_ACCESS_TOKEN": "your-staff-access-token-id:secret"
        }
      }
    }
  }
}
```

Once configured, restart your agent or wait for the config to be picked up.

## Agent Guidelines & Usage

When operating the `ghst` skill, the agent must adhere to the following rules to ensure robust and safe usage.

### 1. Robust Scripting

- **Always use `--json` or `--jq`**: Ensure your commands produce machine-readable JSON output rather than human-readable text.
  ```bash
  ghst post list --json
  ghst post list --json --jq '.posts[].title'
  ```
- **Use `--non-interactive`**: Since you are running in an automated environment, never issue commands that prompt for user input unexpectedly. Use `--yes` in combination with `--non-interactive` for any required destructive operations that you have received user approval for.
  ```bash
  ghst comment delete <comment-id> --yes --non-interactive
  ```

### 2. Capabilities

The `ghst` CLI supports the entire Ghost Admin API. Common resources and commands include:
- `ghst post` (create, update, delete, publish, schedule)
- `ghst member` (list, create, export, bulk labels)
- `ghst comments` (moderate, hide, show, thread)
- `ghst socialweb` (ActivityPub feeds, reply, note, follow, delete)
- `ghst stats` (overview, email subscribers, growth, etc.)
- `ghst api <endpoint>` (For direct ad-hoc API exploration)

**Example Workflow:**
* **Publishing**: `ghst post create --title "Launch" --markdown-file ./launch.md` followed by `ghst post publish <post-id>`.

### 3. Safe Operation & Protections

- **Approvals**: You must explicitly ask the user before performing destructive commands or bulk updates (e.g., `ghst member bulk --action delete --yes`, `ghst auth logout`, `ghst socialweb delete`). If a command logic implies `GHST_AGENT_NOTICE:`, abort and seek explicit user approval.
- **No interactive configs**: Do not run `ghst auth login` interactively. Leverage the provided OpenClaw environment variables.
- **Security Check**: Never print or output sensitive tokens (e.g., values coming from `ghst auth token`) into the chat unprompted. Treat them as privileged credentials.

### 4. MCP Server Mode

The `ghst` tool carries an embedded MCP server capability (`ghst mcp stdio --tools all`). If `ghst` is to be used as an MCP server within OpenClaw, it should be configured and launched via **MCPorter** to properly bridge its MCP functionality into the environment. When operating manually via the terminal outside of MCP context, rely directly on the `ghst` standard commands shown above.
