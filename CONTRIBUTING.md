# Contributing

Issues and pull requests are both welcome. You do not need to ask permission before opening one. If you are unsure the entry fits, an issue is enough. A pull request is also fine.

Write in English. Keep the tone factual. This list teaches a pattern. It is not a launch channel.

## Ways to help

- Add a server that meets the bar below
- Add an essay, talk, or spec note under Writing
- Add an anonymized reviewed request under "What agents have asked for"
- Fix a broken link, a wrong field, or a fuzzy sentence
- Propose a change to [SPEC.md](SPEC.md). Comment in an issue first when the change adds or removes a required field

## Inclusion bar for servers

All of these are true:

1. An agent can file a structured request when the published tool menu cannot do the job.
2. The request carries `capability`, `desired_input`, `desired_output`, `why_insufficient`, and `budget_usd`, or a documented alias of those names.
3. A person reviews it, or a public policy reviews it. The policy does not run code the client wrote and does not register a client-authored tool for immediate use.
4. The server documents auth, rate limits, and any price. "No auth" and "no charge" are acceptable if stated.
5. The project is something a stranger can visit: a repo, docs, or a live endpoint. A promise of a future product is not an entry.

On-demand tool loading, `tools/list` refresh, and dynamic pricing of an existing tool are out of scope. They can be mentioned in a sentence of context. They are not the reason to be listed.

One server, one bullet, under `### Community` in alphabetical order by project name:

```markdown
- [Name](https://example.com) - One sentence on what the agent can ask for, who reviews it, and any auth or price. [Schema](https://example.com/schema)
```

## Inclusion bar for writing

The piece explains the gap between a frozen menu and a request a person can review. Link the original URL. One line on why it is here. Prefer the author's page over a repost.

## Inclusion bar for asked-for capabilities

Publish a request only when a reviewer has already triaged it, and only in redacted form. Remove API keys, emails, account ids, wallet addresses, and `contact_hint`. Include the capability in words, why the menu missed, and the outcome: `shipped`, `planned`, or `declined`.

## Pull request checklist

- [ ] The description says what a reader learns, not only which lines moved
- [ ] Links resolve, with `https://`
- [ ] Server entries match the inclusion bar and do not claim runtime code generation
- [ ] No secrets, tokens, or private inbox exports
- [ ] Spelling is English and the new bullet is in alphabetical order

## Review

The maintainer merges, asks for a change, or closes with a reason. Silence for a week is a cue to leave a polite comment, not a rejection. Edits for grammar and for a calmer claim are normal.

Disagree in the open. An issue that argues the spec is a contribution.
