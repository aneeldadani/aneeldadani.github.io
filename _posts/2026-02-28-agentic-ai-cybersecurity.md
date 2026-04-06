---
layout: post
title: "Agentic AI in Cybersecurity: Moving From Automation to Autonomous Security Workflows"
subtitle: "How human-guided AI agents are reshaping security operations"
date: 2026-02-28
author: Aneel Dadani
tags: [Agentic AI, Cybersecurity, API Security, MCP, AI Agents, Security Operations]
---

*This post is based on my talk "Agentic AI Applications in Cybersecurity."*

Artificial intelligence has been in cybersecurity for years — powering detections, scoring risks, and automating repetitive tasks. But we're in the middle of a meaningful shift. We're moving from AI as a tool you call on to AI as an operational participant — systems that can plan, execute, interpret, and explain security work alongside humans. That's the idea behind agentic AI, and I think it represents a genuinely different model for how security teams can operate.

## The Real Problem Is Orchestration

Security teams don't struggle because they lack tools. They struggle because they can't keep up with orchestrating all of them. Alerts come in from SIEMs, scanners generate findings, endpoints produce telemetry, and APIs surface operational signals. The analyst becomes the integration layer between all of it — gathering context, correlating signals, validating findings, and translating technical output into something the business can act on.

In that sense, analysts are already acting as agents. The limitation isn't capability; it's that human time is finite. Every step of the process — interpreting an alert, deciding which tools to run, executing investigation steps, analyzing outputs, validating findings safely, and explaining risk and remediation — lives inside the analyst's head. Agentic AI tries to externalize parts of that workflow. Not the decision-making authority, but the operational execution that precedes it.

<figure>
  <img src="/assets/img/posts/when-humans-are-the-agent.png" alt="Diagram showing a security analyst as the integration layer between alerts/incidents and tools like SIEM, Scanner, and EDR via APIs">
  <figcaption>When humans are the agent — the analyst orchestrates every tool manually</figcaption>
</figure>

## What Agentic AI Actually Is

Agentic AI is often described as "LLMs with tools," but the real value comes from how the pieces are separated. A well-designed agentic system keeps reasoning and execution deliberately apart.

The agent interface is where goals get defined and workflows get orchestrated. The language model sits in the reasoning layer — it interprets information and generates decisions, but doesn't directly execute anything. Between reasoning and execution sits the Model Context Protocol (MCP), a standardized communication layer that structures requests and keeps things controlled. The MCP server acts as the control boundary: it defines what tools exist, what actions are permitted, and how results come back. Actual work — running scanners, querying alert systems, pulling vulnerability intelligence — happens in the tool execution layer.

The practical result: the LLM reasons, the agent orchestrates, MCP brokers access, and tools execute. The agent never directly touches infrastructure. Every action flows through controlled interfaces, which is what makes this viable in enterprise environments where auditability matters.

<figure>
  <img src="/assets/img/posts/what-is-agentic-ai.png" alt="Architecture diagram showing Claude (LLM) connected to an Agent, which connects via MCP Servers to SIEM, Scanner, and EDR">
  <figcaption>Agentic AI architecture — the LLM reasons, the agent orchestrates, MCP brokers access to tools</figcaption>
</figure>

## 1. Web Application Security Testing

Rather than just running a scanner and dumping a findings list, an agentic workflow simulates how a skilled analyst actually approaches testing. The agent plans a testing strategy, maps the attack surface, executes safe automated checks, interprets findings in context, validates results with non-destructive proof-of-concepts, and produces a remediation plan — all in sequence. The goal isn't to replace scanners; it's to transform scanner output into analyst-quality outcomes.

## 2. Security Alert Triage and Incident Response

A significant chunk of security team time goes to Level 1 triage — work that is largely about assembling context before a human can make any real decision. An agentic system can handle that groundwork: triaging incoming alerts, enriching events with contextual intelligence, correlating related signals, identifying likely false positives, and surfacing credible threats with investigation context already prepared. Humans stay in the decision seat. They just arrive there faster, with better information already in hand.

## 3. Vulnerability Prioritization

Pure CVSS scores are noisy. A critical-rated vulnerability in a system that's unreachable from the internet is less urgent than a medium-rated one in a customer-facing service that's being actively probed. Agentic AI can enrich prioritization by evaluating environmental context, asset importance, reachability, and evidence of active exploitation — so teams get a ranked remediation list that reflects actual exposure rather than theoretical severity.

<figure>
  <img src="/assets/img/posts/demo-architecture.png" alt="Demo architecture diagram showing Claude connected to an MCP Server and Agent Runner (FastAPI), which connects to ZAP scanner, SIEM, and a vulnerable target app">
  <figcaption>A practical demo setup — Claude drives an agent that orchestrates real security tools against a controlled target environment</figcaption>
</figure>

## The Human-in-the-Loop Is the Point

None of this is about removing analysts from the picture. The goal is to change where their expertise gets applied — from collecting information to making informed decisions with context already assembled.

| Traditional Automation | Agentic AI |
|---|---|
| Rule-driven | Goal-driven |
| Static playbooks | Adaptive reasoning |
| Tool outputs | Decision-ready outcomes |
| Humans gather context | Context arrives prepared |

APIs, cloud-native systems, and AI-driven applications have made the operational surface security teams need to cover dramatically larger. You can't scale by just adding headcount. Agentic AI introduces a form of controlled autonomy — systems that perform investigative work safely, explainably, and at a pace that keeps up with modern environments.

## Final Thoughts

The future of security operations isn't fully automated defense. It's augmented operations — humans and intelligent agents working together, where the agent handles preparation and the human handles judgment. The next generation of security tooling won't just generate alerts. It will help us understand them.

If you're thinking about how to apply AI in your security workflows, the key question isn't "how do I automate this?" It's "how do I give my team better context faster?" That's the problem agentic AI is built to solve.

---

*Based on my talk "Agentic AI Applications in Cybersecurity"*
