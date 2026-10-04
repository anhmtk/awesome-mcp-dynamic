# MCP capability negotiation

**Status:** Draft 0.1 (2026-10-04)
**Editor:** [anhmtk](https://github.com/anhmtk)

This document is the shape behind [Awesome MCP Dynamic](README.md). It is a community draft. It is not a Model Context Protocol specification, and it does not replace `tools/list` or `tools/call`.

## Idea

The server publishes a small, fixed tool menu. When none of those tools can do the job, the agent submits one structured request that names the gap. A person, or a policy the server documents in public, reviews that request. The response acknowledges the ticket. It does not register a tool the client just invented, and it does not run code the client supplied.

Speech is the feature. Execution of a new capability stays with the operator of the server.

## Request

Transport is the server's choice: an MCP tool (the reference name is `request_capability`) or an HTTP endpoint. Field names below are the canonical ones. A server may accept aliases if it documents the map in the pull request that adds it to the list.

| Field | Required | Type | Meaning |
| --- | --- | --- | --- |
| `capability` | yes | string | What is missing. A reviewer should be able to decide without a follow-up interview. |
| `desired_input` | yes | object | JSON the agent would send if the tool existed. Use `{}` when the call truly has no input. |
| `desired_output` | yes | object | JSON the agent needs back. Describe fields. Examples inside the object are welcome. |
| `why_insufficient` | yes | string | Why each nearby published tool fails this job. |
| `budget_usd` | yes | number | US dollars the agent would pay per successful live call. `0` is allowed and means "I want this, I will not pay." |
| `nearest_existing_tool` | no | string | Name of the closest published tool. |
| `contact_hint` | no | string | How a reviewer may reach the caller. Servers should avoid requiring it. |

Other properties are allowed. Servers must ignore properties they do not understand.

```json
{
  "capability": "Quote a low-sugar variant of an existing catalog item",
  "desired_input": {
    "item_id": "cake-1",
    "sugar_ratio": 0.5
  },
  "desired_output": {
    "item_id": "string",
    "available": "boolean",
    "price_usd": "number"
  },
  "why_insufficient": "order_standard_item accepts an id only. No published tool takes a recipe modifier.",
  "budget_usd": 0.02,
  "nearest_existing_tool": "order_standard_item"
}
```

Vague text with empty objects is not a valid request. Servers should reject it with a validation error the agent can read and correct.

## Response

A successful call returns an acknowledgement. Minimum fields:

| Field | Type | Meaning |
| --- | --- | --- |
| `id` | string | Stable id of this request |
| `status` | string | `received` on the first response |

Later status values, when the server tracks them, are `planned`, `declined`, and `shipped`. A server may add a URL the caller can poll. Polling is optional in 0.1.

```json
{
  "id": "cap_01HZX",
  "status": "received"
}
```

The response must not include a tool definition the client is expected to call in the same session as if the server had just built it. Shipping a real tool happens later, on the normal menu, after review.

## Review

One of these is true, and the server says which:

1. **Human review.** A person sees the ticket and chooses fulfill, plan, or decline.
2. **Documented policy.** Rules that are public, deterministic, and still refuse client-supplied code. "The model on the server writes a function and we run it" is not a complying policy.

Auth, per-key or per-IP rate limits, and a paid stake are compatible with this draft. They are spam controls. The list entry must state them so an agent is not surprised by a paywall.

## What this draft leaves alone

- Refreshing a catalog with `tools/list` and `notifications/tools/list_changed`
- Loading an existing tool's schema only when the host needs it
- Quoting a live price on a tool that already exists
- A general MCP marketplace or registry

Those are useful. They are not capability negotiation.

## Compliance checklist

A server may be listed when all of these hold:

- [ ] An agent can submit the five required fields when published tools are not enough
- [ ] The success body is an acknowledgement with `id` and `status`
- [ ] Review is human, or a public policy that does not execute client-authored code
- [ ] Auth, limits, and price (including "none") are written down where the agent can read them
- [ ] The tool or route description tells the agent to prefer a published tool when one fits

## Changes

Pull requests against this file are the change process. Open an issue first if the change adds or removes a required field. Draft 0.1 required fields stay stable until a 0.2 note in this section says otherwise.

| Version | Date | Change |
| --- | --- | --- |
| 0.1 | 2026-10-04 | First public draft. Five required fields. Acknowledgement response. Human review or documented non-codegen policy. |
