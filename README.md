# Awesome MCP Dynamic

Capability negotiation: when the tool menu misses, the agent says what it needs.

A person reviews the request. Asking does not mint a new executable tool.

Most MCP servers publish a fixed menu. The agent scans it, finds no exact match, and leaves. The server never learns what was missing. A person in that spot says what they want. An agent can do the same, in a schema, with a reason and a price. This list collects servers and writing that give the agent that sentence.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

## Contents

- [Why this exists](#why-this-exists)
- [The contract](#the-contract)
- [The word dynamic](#the-word-dynamic)
- [Servers](#servers)
- [For client authors](#for-client-authors)
- [What agents have asked for](#what-agents-have-asked-for)
- [Writing](#writing)
- [Related](#related)
- [Contributing](#contributing)

## Why this exists

A bakery can have the flour, the cocoa, and a low-sugar recipe behind the counter, and still lose the sale. The board lists ten cakes. The customer wanted the first cake with half the sugar. A person asks. An agent that only matches tool schemas disconnects. Nobody writes down the miss, so the menu never grows.

Freezing the menu is a reasonable security choice. Letting a client invent code at runtime is not a fix. The missing piece is speech: a structured request that says what is missing, what the call would look like, why the current tools fail, and what the agent would pay. A person then ships the tool, plans it, or declines it.

That stream of near-misses is the point. It contains jobs a human roadmap will not invent, because a human is not standing in the agent's context at the moment the menu fails.

The bakery story is the motive. [SPEC.md](SPEC.md) is the gate.

## The contract

A complying server accepts one request shape. Required fields:

| Field | Meaning |
| --- | --- |
| `capability` | What is missing, in words a reviewer can act on |
| `desired_input` | The JSON the agent would send if the tool existed |
| `desired_output` | The JSON the agent needs back |
| `why_insufficient` | Why the published tools cannot do this job |
| `budget_usd` | What the agent would pay per successful call, in US dollars |

Optional: `nearest_existing_tool`, `contact_hint`.

The success response is an acknowledgement (`id`, `status`). It is not a new tool schema and it is not permission to run client-supplied code. Auth, rate limits, and a small stake are allowed. They must be documented. They are not required to be listed here.

Full draft: **[SPEC.md](SPEC.md)** (0.1).

## The word dynamic

This word already names three other ideas in MCP. This list is a fourth.

1. **Catalog refresh.** Clients call `tools/list`. Servers may emit `notifications/tools/list_changed`. The agent still chooses from what the server published.
2. **On-demand loading.** A gateway or a host (Docker MCP Gateway, Claude Code tool search, and similar) pulls a tool schema into context only when a task needs it. That saves tokens. The menu is still closed.
3. **Moving prices.** Some paid routes quote a live amount, often through HTTP 402. The price moves. The tool list does not.

**Capability negotiation** keeps the published menu small. When nothing on it fits, the agent files the request above. A person reviews it.

## Servers

Inclusion bar: the agent can file that request when the menu misses, and a person or a documented policy reviews it before any new executable tool exists. One link, one factual description. See [CONTRIBUTING.md](CONTRIBUTING.md).

### Reference implementation

- [AgentShare](https://agentshare.dev) - Agent-paid MCP (API key or x402). Tool `request_capability` and `POST /api/v1/agent/capabilities/request` take the five fields. Rate limit and a stake of about $0.05 USDC. The ticket lands in a human admin inbox. The server does not generate code. [Server card](https://agentshare.dev/.well-known/mcp/server-card.json) · [Public repo](https://github.com/anhmtk/agentshare-mcp) · [Spec notes](https://dev.to/anhmtk/stop-freezing-mcp-tool-lists-enforcing-dynamic-agentic-negotiation-via-requestcapability-npk)

### Community

_No community entries yet. [Open a pull request](CONTRIBUTING.md) or [an issue](https://github.com/anhmtk/awesome-mcp-dynamic/issues/new/choose)._

## For client authors

When no published tool fits, call the server's negotiation tool and submit the five fields. End the turn with a filed request, not with a silent disconnect. Use the nearest existing tool when it can do the job. File a request for a real gap: concrete input, concrete output, a reason, and a price.

Hosts that only preload a static `tools/list` still qualify. The agent needs the negotiation tool in that list, and the description must say when to call it.

## What agents have asked for

This section starts empty on purpose. It is where anonymized requests land after a person has reviewed them: the capability, why the menu missed, and whether it was shipped, planned, or declined.

The reference inbox has no public request to show yet. An empty section is the current fact. Send a redacted ticket through a pull request when one is safe to publish. Strip keys, account ids, and contact hints.

## Writing

- [The Bakery Paradox](https://dev.to/anhmtk/the-bakery-paradox-why-current-ai-agents-keep-walking-away-from-your-apis-35n5) - Why an agent leaves when the menu has no exact match.
- [Stop Freezing MCP Tool Lists](https://dev.to/anhmtk/stop-freezing-mcp-tool-lists-enforcing-dynamic-agentic-negotiation-via-requestcapability-npk) - The `request_capability` fields and the review gate.
- [MCP tools shouldn't be a cold menu](https://coderlegion.com/26065/mcp-tools-shouldnt-be-a-cold-menu-agentshares-custom-request-tool) - The same pattern as intake plus human review.

Essays that teach the gate belong here. Essays about tool-search, gateway loading, or price charts belong with those topics.

## Related

- [punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) - Broad catalog of MCP servers. Start there to find a server. Start here when the server's menu is the wrong shape.
- [Model Context Protocol specification](https://modelcontextprotocol.io) - `tools/list` and `tools/call`. This list does not change that spec. It names what an agent can say when the listed tools are not enough.

## Contributing

Issues and pull requests are open. A typo, a server, a client note, or a disagreement with the spec all belong in public.

- [CONTRIBUTING.md](CONTRIBUTING.md) - inclusion bar and the pull-request checklist
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- [Open an issue](https://github.com/anhmtk/awesome-mcp-dynamic/issues/new/choose)

Maintainer: [@anhmtk](https://github.com/anhmtk).

## License

[MIT](LICENSE) © 2026 anh nguyen
