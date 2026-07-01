# pylon-connector

A Claude plugin that lets Claude read solar-proposal data from the **Pylon** web
app (`app.getpylon.com`) and push those projects into the **Formbay** job queue —
or look up jobs already in the Formbay portal.

## What it does

The plugin bundles one skill, **`pylon-scraper`**, which activates whenever the
target site is `app.getpylon.com` or the user mentions Pylon / getpylon / STCs /
"push to Formbay" / Formbay jobs. It:

- **Reads Pylon projects** (read-only) from the rendered DOM via the Claude in
  Chrome browser tools — primarily project **address** and **number of STCs**,
  plus system size, price, and panel model. Pylon is a Vue SPA with no public
  JSON API, so the skill scrapes the rendered page rather than calling an
  `/api/` endpoint.
- **Pushes a project into the Formbay job queue** as an installation job, or
  **looks up existing jobs**, via the `fb-job-push` MCP server. Looking jobs up
  (`list_jobs` / `get_job` / `whoami`) is read-only; `push_job` is a **write**
  action and is always confirmed with the exact payload before it runs.

## Requirements

- **Claude in Chrome** browser tools, with a browser already logged in to Pylon.
- Access to the Formbay **`fb-job-push`** MCP server (declared in
  [.mcp.json](.mcp.json)).

The `fb-job-push` MCP server is configured in [.mcp.json](.mcp.json) at
`https://mcp.13-236-107-181.nip.io/mcp`. It is authenticated — you need valid,
active Formbay credentials for the job tools (`list_jobs` / `get_job` /
`push_job` / `whoami`) to work.

## Layout

```
pylon-connector/
├── .claude-plugin/plugin.json     # plugin manifest
├── .mcp.json                      # fb-job-push MCP server (URL TBD)
├── skills/
│   └── pylon-scraper/SKILL.md     # the skill
└── README.md
```

## Access

Restricted to authorized Formbay customers. Every Formbay action runs through the
authenticated Formbay MCP server. See the repository [LICENSE](../../../LICENSE).
