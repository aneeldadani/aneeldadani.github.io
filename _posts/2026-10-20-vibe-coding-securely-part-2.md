---
layout: post
title: "Vibe Coding Securely, Part 2 of 3: Placeholders and Secrets"
date: 2026-10-20
author: Aneel Dadani
tags: [AppSec, Vibe Coding, AI Security, Secure Coding]
---

In [Part 1](/2026-10-13-vibe-coding-securely-part-1/) I covered two habits: giving your AI agent standing security context, and reviewing every change it makes to auth. This time I'll cover two problems that make AI-written code look finished when it isn't.

## Habit 3: Hunt for placeholders

When an agent can't finish something, it often writes an incomplete version so the code still runs. That can look like a check that always passes, a TODO where validation should be, or middleware that's commented out "temporarily." The app still works, the demo looks great, and the security check was never written.

**Rule:** "Never ship incomplete code or skip a security check. If you can't complete it, deny access and tell me."

**Check:** fail the build if TODO, FIXME, or a hardcoded return shows up in auth, payment, or access control code.

**Example:** an `isAuthorized()` function that just returns `true`. It passes every test that only checks the happy path.

The principle is fail closed. If the agent isn't sure, the default should be no access, not full access.

## Habit 4: Keep secrets out of the code

Agents want the code to run, and the fastest way to make an API call work is to paste the key right into it.

**Rule:** "Read secrets only from environment variables. Never write a key, token, or password into code, config, or tests."

**Check:** run a secret scan before every commit, so a slip gets blocked before it's pushed.

**Example:** a `.env` file that never made it into `.gitignore`, or a privileged API key placed in frontend code where anyone can read it in the browser.

If a secret does get committed, deleting it isn't enough. It's still in your git history. Rotate it.

## Rules and a pre-commit hook

The agent rules and a tested pre-commit hook that enforces both habits are in this [gist](https://gist.github.com/aneeldadani/82294342989844094055873d85e6415e).

**Previous:** [Part 1: Context and Review](/2026-10-13-vibe-coding-securely-part-1/)

**Next:** [Part 3: Dependencies and Attacker Testing](/2026-10-27-vibe-coding-securely-part-3/)
