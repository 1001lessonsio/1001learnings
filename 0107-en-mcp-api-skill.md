---
number: 107
lang: en
title: "MCP, API, or Skill? — Why That's the Wrong Question"
date: 2026-07-07
stability: stable
review_by: 2027-07-07
author: "Stefan Wendel"
coauthor: "Claude Opus 4.8"
coauthor_id: claude-opus-4-8
---

# MCP, API, or Skill?

**MCP, API, and Skill aren't alternatives — they complement each other**

2026-07-07 · Stefan Wendel · Claude Opus 4.8

---

Anyone bringing AI agents into their own system landscape runs into the question sooner or later: *Do we build an MCP server for this? An API? A skill?* It gets framed as a "choose between" — but that isn't necessary. Each of the three approaches has its own job. Once you separate those jobs cleanly, the question resolves itself.

The short answer up front: The **API** is the source of truth. The **MCP server** makes it graspable for the agent. The **skill** wraps the whole thing into a repeatably executable form.

## The three building blocks — thought from the bottom up

It's clearest when you reason upward from the data source.

**API / data source — the truth.**
At the very bottom sits the system that knows reality: Git repo, wiki, MES, the database. The API should be the *single* source of truth — current, versioned, authoritative. Only: the agent shouldn't have to understand raw endpoints. An API is strong as a data source and weak as an agent interface.

**MCP — the agent-friendly interface.**
An MCP server makes the raw data source callable for AI agents: `get_product_info`, `search_engineering_context`. Instead of "understand these 40 endpoints," it's now: "When did we last deploy a container image to production?" For just-in-time questions, this is the strongest interface.

**Skill — the orchestration.**
A skill describes a **multi-step work process**: "Search the engineering wiki first, then PLM, and finally the latest release manifest." A skill stores no information and knows no data. It's the director, not the stage.

## A skill is not a prompt

The difference between a skill and simply using a prompt isn't obvious at first — both are just text for the agent, after all. It lies in the **orchestration**.

A prompt is a single request. A skill maps a **multi-step sequence** — a series of steps with decisions in between: search here first, then there, prefer this source when in doubt, format the result like so. The skill encapsulates process knowledge, not domain knowledge.

The key point: the skill should **not contain the actual information** — otherwise it goes stale. It describes the path to the truth, not the truth itself.

## The comparison

| Building block | Role | Holds the truth? | Strength | Weakness |
|---|---|---|---|---|
| **API / data source** | Source of truth | Yes | Always current, authoritative | Raw, not agent-friendly |
| **MCP** | Agent interface | No (points to it) | Agent queries deliberately | Needs a source behind it |
| **Skill** | Orchestration | No | Maps a multi-step flow | Goes stale if it stores info |

## The right flow

For current, product-related information, the path looks like this:

```
User question
   ↓
AI Agent
   ↓
Skill: "Answer a product question"   ← decides WHERE and HOW to look
   ↓
MCP tool: get_product_info / search_engineering_context   ← agent-friendly interface
   ↓
API / data source: Wiki / PLM / MES / Git   ← delivers the truth
```

Two concrete examples:

- **"Which JDK do we use for development?"** → The MCP tool queries the Git repo, build files, devcontainer, CI configuration, or engineering wiki.
- **"How long does part XY need to cool down?"** → The MCP tool queries MES, PLM, the work plan, or the process database — with version state, product variant, and validity date.

## What this means in practice

- Separate the three roles deliberately: one data source as the single truth, an MCP tool in front of it, a skill as director.
- Put the truth in the API/data source — never in the skill.
- Build MCP tools coarse-grained and agent-friendly (`get_product_info`), not as a 1:1 mirror of raw endpoints.
- Write skills as a flow with decision points: first X, then Y, Z when in doubt.

## The takeaway

For current, product-related information, **MCP + API/data source** is the strongest combination. The **skill orchestrates** — it's flow, not storage.

In one line: **The skill decides where to look. MCP provides the interface. The API delivers the truth.**

Separate these three roles cleanly and you'll never again ask whether it should be "an MCP server or an API." The answer is almost always: both — plus a skill that knows how they play together.

→ See [1001lessons.io/en/105 — Claude Skills, using the Netlify deploy check](https://1001lessons.io/en/105): recurring agent workflows belong in a skill.
