---
layout: post
title: "Vibe Coding Securely, Part 1 of 3: Context and Review"
date: 2026-10-13
author: Aneel Dadani
tags: [AppSec, Vibe Coding, AI Security, Secure Coding]
---

If you're vibe coding with an AI agent, here are the techniques I'd use to do it securely. AI coding agents have gotten better, but they still ship vulnerabilities.

In one study, researchers had five AI coding agents build the same apps from the same prompts. Across 15 apps, they found 69 vulnerabilities. Zero exploitable SQL injection or XSS. The most common critical issue was broken authorization and business logic.

An agent can write a safe database query. It can't know that a user shouldn't see another customer's invoice unless you tell it. And in a long session, it can forget what you told it an hour ago.

This series covers six habits for vibe coding without shipping those gaps. Each one follows the same pattern: write the rule down, then enforce it with a check that doesn't depend on the agent remembering.

## Habit 1: Give the agent standing security context

A prompt covers one request. Your agent's instruction file is read every session. Put your security rules there:

- Roles, and who can access what
- Which data is sensitive and must never be logged or returned
- Patterns it must always use: ownership checks on every object ID, parameterized queries, secrets from environment variables
- Patterns it must never use: hardcoded credentials, disabling auth "for testing"
- Approved libraries for auth, cryptography, and file handling

Here's what that looks like in practice:

> **Always:** check object ownership on every endpoint that takes an ID. Deny by default.
>
> **Never:** hardcode credentials or disable auth, even "for testing."
>
> **When you finish:** list every change that touches auth or sensitive data.

Treat that file like code and review every change to it. A malicious edit can quietly tell the agent to write insecure code.

## Habit 2: Review everything that touches authentication and access control

This is where agents fail most, so it's where your attention goes first.

**Rule:** "List every authentication or access control change in your summary."

**Check:** nothing that touches auth gets merged without a human review.

**Example:** the agent adds `GET /api/invoices/:id`. Being logged in isn't enough. Confirm it checks that the invoice belongs to the user asking for it.

## A starter rules file

Drop this section into `AGENTS.md`, `CLAUDE.md`, or your agent's rules file, then customize the roles, data, and libraries for your app. Rules guide the agent; back them with tests and CI checks that enforce them.

```markdown
## Security rules (apply to every task)

### Roles and access
- Roles: admin, member, viewer
- Members can only read or change records where record.org_id matches their org
- Only admins can manage users, billing, and API keys

### Sensitive data
- Never log or return: passwords, API keys, tokens, card numbers, SSNs
- Strip internal IDs and owner fields from public API responses

### Always
- Check object ownership on every endpoint that takes an ID, using the shared authorization middleware
- Use parameterized queries. No string-built SQL
- Read secrets from environment variables only
- Deny access by default if a check can't be completed

### Never
- Hardcode credentials, keys, or test users
- Disable or bypass auth, even temporarily or "for testing"
- Add a dependency without asking first

### Approved libraries
- Auth: [your auth library]
- Cryptography (password hashing, encryption, token signing, random values): [your library]. Never write custom crypto
- File uploads: [your upload handler]

### When you finish a task
- List every change that touches authentication, authorization, or sensitive data
- List any new dependencies
```

It's also available as a [gist](https://gist.github.com/aneeldadani/baa3b9bcaef157605f92a042a16b118b).

**Next:** [Part 2: Placeholders and Secrets](/2026-10-20-vibe-coding-securely-part-2/)
