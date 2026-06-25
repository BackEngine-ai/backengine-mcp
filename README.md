<p align="center">
  <img src="assets/logo.png" alt="BackEngine" width="128" height="128">
</p>

<h1 align="center">BackEngine MCP Server</h1>

<p align="center">
  The customer-context layer for your revenue team — available to Claude and any MCP client.
</p>

## What is BackEngine?

BackEngine is a multi-tenant SaaS platform that ingests all of an organization's customer
and prospect communications — Slack threads, emails, call and meeting transcripts, and support
tickets — and distills them into structured, queryable context. Raw conversations are processed
into **signals** (atomic, attributed observations), **sources** (the underlying transcripts,
emails, and tickets), and rolling **account overviews**, all isolated per tenant and scoped by
role and access controls. The MCP server exposes this layer over the
[Model Context Protocol](https://modelcontextprotocol.io), so an LLM client can query customer
signals, reconstruct email threads, prep for upcoming meetings, surface at-risk accounts, run
reports, and draft outreach grounded in real conversation history — without leaving the chat.

## Connecting

The server is remote and speaks streamable HTTP at **`https://mcp.backengine.ai/sse`**. You'll
need a BackEngine account; the server authenticates your session on connect.

### Clients with native remote MCP support

```json
{
  "mcpServers": {
    "backengine": {
      "type": "streamable-http",
      "url": "https://mcp.backengine.ai/sse"
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
      "args": ["-y", "mcp-remote", "https://mcp.backengine.ai/sse"]
    }
  }
}
```

## Tools

The server exposes the following tools. Most are read/query tools that ground the model in your
customer data; a smaller set writes to brains, subscriptions, and Slack.

### Search & discovery

| Tool | Description |
| --- | --- |
| `ask_backengine` | Find the BackEngine artifact (playbook, report template) best suited to answer a free-text question. |
| `find_similar_signals` | Find signals matching a natural-language query. |
| `find_similar_sources` | Find sources matching a natural-language query. |
| `find_similar_tickets` | Find tickets matching a natural-language query. |
| `find_contacts` | Find contacts by name, email, job title, customer, or prospect. |
| `find_nodes` | Find nodes in a brain by type and/or field predicates. |

### Accounts & projects

| Tool | Description |
| --- | --- |
| `list_projects` | List projects (customers or prospects). |
| `get_project` | Retrieve information about a project (customer or prospect). |
| `get_project_overview` | Retrieve an account overview built from the last 12 weeks of data. |
| `list_project_groups` | List all project groups. |
| `get_project_group` | Retrieve information about a project group. |
| `get_activity` | Count signals and sources over a date range. |

### Signals & sources

| Tool | Description |
| --- | --- |
| `list_signals` | List signals. |
| `get_signal` | Retrieve a single signal. |
| `list_signal_speakers` | List the speakers attributed across signals. |
| `list_sources` | List sources (transcripts, emails, tickets, etc.). |
| `get_source` | Retrieve a single source. |
| `get_email_thread` | Retrieve and reconstruct a full email thread. |

### Tickets

| Tool | Description |
| --- | --- |
| `list_tickets` | List tickets. |

### Calendar & meetings

| Tool | Description |
| --- | --- |
| `get_calendar_events` | Retrieve calendar events for the current user. |
| `get_team_calendar_events` | Retrieve calendar events for a team over a date range. |

### Reports, templates & playbooks

| Tool | Description |
| --- | --- |
| `list_reports` | List generated reports. |
| `get_report` | Retrieve a generated report. |
| `list_report_templates` | List available report types. |
| `list_templates` | List available templates. |
| `get_template` | Retrieve a template. |
| `list_playbooks` | List available playbooks. |
| `get_playbook` | Retrieve a playbook's body (prompt). |

### Usage analytics

| Tool | Description |
| --- | --- |
| `list_usage_measures` | Discover available usage measures and the axes they can be cut by. |
| `query_usage` | Aggregate recorded product-usage over a date range. |

### Brains & nodes

| Tool | Description |
| --- | --- |
| `list_brains` | List brains you can access. |
| `get_brain` | Get a brain's details and types. |
| `create_brain` | Create a new brain (a tweakable, optionally-typed node store). |
| `update_brain` | Update a brain's name, description, or scope. |
| `add_type` | Add a type (the schema for a kind of node) to a brain. |
| `update_type` | Update a type's schema. |
| `create_node` | Create a node in a brain. |
| `get_node` | Retrieve a single node from a brain. |
| `update_node` | Update a node's field values. |
| `link_nodes` | Link two nodes with a labeled relationship. |

### People & teams

| Tool | Description |
| --- | --- |
| `list_users` | List all users. |
| `get_user` | Look up a user by name, email, or id. |
| `list_user_groups` | List user groups (teams). |
| `get_user_group` | Retrieve information about a user group (team). |
| `list_roles` | List active roles. |
| `get_role` | Retrieve information about a role. |

### Subscriptions

| Tool | Description |
| --- | --- |
| `list_subscriptions` | List subscriptions. |
| `get_subscription` | Retrieve detailed information about a subscription. |

### Integrations

| Tool | Description |
| --- | --- |
| `list_services` | List integrated services. |
| `list_channels` | Retrieve available Slack channels. |
| `send_slack_message` | Send a Slack message. |
| `list_hubspot_fields` | Retrieve available HubSpot fields. |
| `list_hubspot_owners` | Retrieve HubSpot owners. |
| `list_hubspot_scopes` | Retrieve the current HubSpot scopes. |

### Platform & navigation

| Tool | Description |
| --- | --- |
| `get_context` | Retrieve a saved context (working set / scope). |
| `list_contexts` | List available contexts. |
| `get_navigation_url` | Get a deep-link URL into the BackEngine app. |
| `list_audit_events` | List events from the internal audit log. |
| `list_backengine_releases` | List BackEngine releases and versions. |

## Links

- Website: https://backengine.ai
- MCP endpoint: https://mcp.backengine.ai/sse
- Model Context Protocol: https://modelcontextprotocol.io
