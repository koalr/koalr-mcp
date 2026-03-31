# koalr-mcp

**Koalr MCP server** — connect your engineering metrics, deploy risk scoring, and DORA performance data to Claude, Cursor, Windsurf, and any MCP-compatible AI assistant.

[![npm version](https://img.shields.io/npm/v/koalr-mcp)](https://www.npmjs.com/package/koalr-mcp)
[![smithery badge](https://smithery.ai/badge/koalr-mcp)](https://smithery.ai/server/koalr-mcp)
[![MIT License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

---

## What can it do?

Once connected, ask your AI assistant things like:

- *"Score PR #847 in our API repo for deployment risk"*
- *"Which open PRs are most likely to cause a production incident?"*
- *"What is our DORA performance tier this month?"*
- *"Show me all PRs open more than 3 days on the backend team"*
- *"How is our AI coding tool adoption trending?"*
- *"What was our MTTR for incidents last quarter?"*
- *"Find developers with the highest PR throughput"*
- *"What is our org-wide test coverage trend?"*

## Quick Start

### 1. Get an API Key

Go to [app.koalr.com/settings/api-keys](https://app.koalr.com/settings/api-keys), create a key, and copy it. It starts with `koalr_`.

### 2. Install in your AI assistant

#### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

#### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

#### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "koalr": {
      "command": "npx",
      "args": ["-y", "koalr-mcp@latest"],
      "env": {
        "KOALR_API_KEY": "koalr_your_key_here"
      }
    }
  }
}
```

#### Claude Code (CLI)

```bash
claude mcp add koalr \
  -e KOALR_API_KEY=koalr_your_key_here \
  -- npx -y koalr-mcp@latest
```

---

## Available Tools

### Deploy Risk

| Tool | Description |
|---|---|
| `score_pr_for_deploy_risk` | Score a PR 0–100 for deployment risk using Koalr's 36-signal model. Returns factor breakdown: change entropy, DDL migrations, author expertise, CODEOWNERS violations, and more. |

### Organization

| Tool | Description |
|---|---|
| `get_org_health` | Overall org health: DORA tier, cycle time, active teams, recent incidents |
| `get_well_being_summary` | Developer well-being: focus time, meeting load, burnout signals |

### DORA Metrics

| Tool | Description |
|---|---|
| `get_dora_summary` | Deployment frequency, lead time for changes, change failure rate, MTTR |
| `get_dora_trend` | DORA trend over time (7d, 14d, 30d, 90d) |

### Pull Requests

| Tool | Description |
|---|---|
| `get_pr_summary` | PR volume, cycle time, review turnaround for a period |
| `get_open_prs` | Currently open PRs with age, size, and review status |
| `get_at_risk_prs` | PRs flagged as at-risk: long open, large, stalled in review |

### Teams

| Tool | Description |
|---|---|
| `list_teams` | All teams in your org |
| `get_team` | Team detail: members, DORA performance, recent activity |
| `list_team_members` | Members of a specific team |

### Developers

| Tool | Description |
|---|---|
| `get_developer` | Individual developer profile: PR throughput, review activity, cycle time |
| `list_top_contributors` | Top contributors by PR throughput, reviews, or impact |

### Repositories

| Tool | Description |
|---|---|
| `list_repositories` | All repos with language, activity, and health status |
| `get_repository` | Single repo detail: deployment frequency, cycle time, contributors |

### Incidents

| Tool | Description |
|---|---|
| `list_recent_incidents` | Recent incidents from PagerDuty/OpsGenie with MTTR and severity |

### Coverage

| Tool | Description |
|---|---|
| `get_coverage_summary` | Test coverage by repository with trend |

### AI Adoption

| Tool | Description |
|---|---|
| `get_ai_adoption_summary` | GitHub Copilot and Cursor usage metrics |
| `get_ai_adoption_trend` | AI coding tool adoption trend over time |

### Search

| Tool | Description |
|---|---|
| `search` | Search developers, repos, PRs, and teams by name |

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `KOALR_API_KEY` | Your Koalr API key (required) | — |
| `KOALR_API_URL` | API base URL (for self-hosted Koalr) | `https://api.koalr.com` |
| `MCP_TRANSPORT` | Transport mode: `stdio` or `http` | `stdio` |
| `PORT` | Port for HTTP transport mode | `3010` |

---

## About Koalr

[Koalr](https://koalr.com) is an engineering metrics platform for software teams. It tracks DORA metrics, PR health, developer experience, and — uniquely — **deployment risk prediction** using a 36-signal ML model that scores every PR before it merges.

[Sign up free](https://app.koalr.com/signup) · [Docs](https://docs.koalr.com) · [koalr.com](https://koalr.com)
