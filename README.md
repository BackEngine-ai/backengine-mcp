<p>
  <img src="assets/logo.png" alt="BackEngine" width="128" height="128">
</p>

<h1>BackEngine MCP Server</h1>

<p>
  The customer-context layer for your revenue team — available to Claude and any MCP client.
</p>

## What is BackEngine?

BackEngine is a multi-tenant SaaS platform that ingests all of an organization's customer
and prospect communications — Slack threads, emails, call and meeting transcripts, and support
tickets — and distills them into structured, queryable context. Raw conversations are processed
into **signals** (categorized, attributed moments), **sources** (the underlying transcripts,
emails, and tickets), and rolling **project overviews**, all isolated per tenant and scoped by
role and access controls. The MCP server exposes this layer over the
[Model Context Protocol](https://modelcontextprotocol.io), so an LLM client can query customer
signals, reconstruct context, prep for meetings, surface at-risk accounts, and draft grounded
outreach from real conversation history — without leaving the chat.

## Connecting

The server is remote and speaks streamable HTTP at **`https://mcp.backengine.ai/mcp`**. You'll
need a BackEngine account; the server authenticates your session on connect.

### Clients with native remote MCP support

```json
{
  "mcpServers": {
    "backengine": {
      "type": "streamable-http",
      "url": "https://mcp.backengine.ai/mcp"
    }
  }
}
```

### Clients that require a stdio bridge

For clients that don't yet support remote servers directly, proxy through
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "backengine": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.backengine.ai/mcp"]
    }
  }
}
```

## Tools

The core tools for querying customer and prospect context are listed below. The exact tool set
is resolved per connection — additional tools (creating and updating records, and
integration-specific actions for Jira, Zendesk, HubSpot, Salesforce, and Slack) become available
based on your permissions and which integrations your workspace has connected.

| Tool | Description |
| --- | --- |
| `list_projects` | List customers and prospects; filter by name, type, etc. |
| `get_project` | Retrieve a customer or prospect, optionally with its overview. |
| `get_project_overview` | Pre-computed rolling 12-week summary for a customer or prospect. |
| `list_signals` | List signals (categorized moments); filter by project, role, and time. |
| `find_similar_signals` | Semantic search across signals from a natural-language query. |
| `list_sources` | List sources — transcripts, emails, Slack threads, and tickets. |
| `get_source` | Retrieve a source's content (summary, verbatim, or metadata only). |
| `list_roles` | List the perspective lenses (e.g. product, sales, CS) used to filter signals. |
| `find_contacts` | Find contacts by name, email, job title, or project. |
| `list_signal_speakers` | List who has been speaking across signals. |

## Links

- Website: https://backengine.ai
- MCP endpoint: https://mcp.backengine.ai/mcp
- Model Context Protocol: https://modelcontextprotocol.io
