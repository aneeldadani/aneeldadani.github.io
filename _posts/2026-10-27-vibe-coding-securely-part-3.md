---
layout: post
title: "Vibe Coding Securely, Part 3 of 3: Dependencies and Attacker Testing"
date: 2026-10-27
author: Aneel Dadani
tags: [AppSec, Vibe Coding, AI Security, Secure Coding]
---

[Part 1](/2026-10-13-vibe-coding-securely-part-1/) covered giving your AI agent security context and reviewing auth changes. [Part 2](/2026-10-20-vibe-coding-securely-part-2/) covered placeholders and secrets. This time: what your agent installs, and testing your app the way an attacker would.

## Habit 5: Check what it installs

Agents solve problems fast by pulling in packages. That creates two risks.

The first is age. Agents suggest packages and versions from their training data, so they'll happily install a version with known vulnerabilities, or a library nobody has maintained in years.

The second has a name: slopsquatting. AI models sometimes recommend packages that sound real but don't exist. In one study of 16 models, about 1 in 5 recommended packages didn't exist, and 43% of the fake names came back every time the same prompt was rerun [[1]](#ref-1). That repetition is what attackers exploit. They collect the names models keep inventing, register them with malicious code, and wait. The next agent that suggests one installs the attacker's code, and install scripts can run before you ever open the app.

**Rule:** "Ask before adding any new dependency. Only use packages from the approved list, and pin versions."

**Check:** fail the build when the lockfile adds a package that isn't approved, and scan dependencies for known vulnerabilities on every change.

**Example:** a package name one letter off from a popular library, or one that didn't exist until someone registered the name an AI made up.

## Habit 6: Test like an attacker

Agents test the happy path. Their tests prove a feature works, not that it fails safely when someone abuses it. So write the tests an attacker would.

**Rule:** "For every endpoint that accepts input, write abuse tests: extra fields, steps out of order, and values that shouldn't be allowed."

**Check:** those tests run on every change, and a failing abuse test blocks the merge.

**Example:** your checkout sends the product, quantity, and price from the browser. An attacker changes the price from $199 to $0.01. If the agent wrote code that charges whatever price the request says, the order goes through for a penny. The fix: the server looks up the price itself and ignores what the browser sends. Then test a quantity of -1.

## Rules, a dependency check, and attacker tests

The agent rules, a dependency check, and example attacker tests are in this [gist](https://gist.github.com/aneeldadani/b8e992dc667d9e3d4d633ecb5d914265).

## The series in one list

Six habits, one pattern: write the rule down, then enforce it with a check that doesn't depend on the agent remembering.

1. Give the agent standing security context
2. Review everything that touches auth
3. Hunt for placeholders
4. Keep secrets out of the code
5. Check what it installs
6. Test like an attacker

Vibe coding gets you to a working app fast. These habits keep it secure once real users show up.

## References

<a id="ref-1"></a>[1] Spracklen et al., "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs," USENIX Security 2025. [arXiv:2406.10279](https://arxiv.org/abs/2406.10279)

**Previous:** [Part 2: Placeholders and Secrets](/2026-10-20-vibe-coding-securely-part-2/)
