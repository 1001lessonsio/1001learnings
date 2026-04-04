---
number: 106
lang: en
title: "The Forgetful Craftsman — How to Teach AI Agents to Remember Their Own Tools"
date: 2026-04-04
stability: volatile
review_by: 2027-04-04
updated: 2026-04-04
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# The Forgetful Craftsman — How to Teach AI Agents to Remember Their Own Tools

**And why organizations need a more rigorous approach than individual developers.**

2026-04-04 · Stefan Wendel · Claude Opus 4.6

---

Think of the best craftsman you know. He can build anything — furniture, plumbing, electrical work. But every morning, when he shows up at your house, he's forgotten where his tools are. He doesn't even remember using a power drill yesterday. So he improvises. Screws things in by hand. Or walks out to buy a new one.

This is not a hypothetical. This is Claude Code, Codex, opencode — whichever AI agent your organization relies on — after a restart. Right when you need it most. When the dishwasher is flooding, the server is down, the client is on the phone.

## The PDF Debacle

In a morning session, I needed a PDF from a Markdown document. Claude found an elegant solution: a small Python script, `fpdf2` and `markdown` as dependencies, done. Clean, transparent, worked on the first try. I was impressed.

That afternoon — new session after a context clear — I needed another PDF. Same machine, same repo, same Markdown. What followed was a wild spree of installation attempts: downloading LaTeX, searching for pandoc, trying random npm packages. The Python script from that morning? Still there. Still in the same directory. But the agent had no idea it existed.

That was the moment I realized: **The problem isn't the agent. The problem is me.** I hadn't written down the working recipe — at least not in a way the agent could find it in the next session.

## The Forgetful Craftsman — and What Cures Him

The craftsman isn't stupid. He doesn't need a better brain or more training — he's had plenty of both. He needs a system: tools in a fixed place, instructions on the wall, a checklist every morning. In short: good habits.

For AI agents, such habits exist. Four of them — each solving its own problem.

## Habit 1: CLAUDE.md — the Blueprint on the Wall

`CLAUDE.md` is a file the agent reads at the start of every session. It's his memory — or more precisely: his substitute for one.

There are two levels:
- **Global** (`~/.claude/CLAUDE.md`): Applies to every project on the machine. This is where universal recipes go — for example: *"PDFs are generated exclusively with `~/.claude/md_to_pdf.py`. No LaTeX, no pandoc, no installing new tools."* Think of it as the master plan for the entire building project. How do the rooms connect? What comes in which order? Who's responsible for what?
- **Per repo** (`<repo>/CLAUDE.md`): Project-specific behavior. Git conventions, deployment processes, test strategies. This is the blueprint pinned to the wall of the room you're currently working in.

My PDF problem was solved in 30 seconds once I wrote that sentence into the global `CLAUDE.md` (or rather: had it written). Since then, it's clear how PDFs are supposed to be generated.

**The lesson:** If the agent did something right today, have it written down before you close the session. Not as documentation for humans — as an instruction for the agent.

And be explicit about *where* it gets written down: *"Please note in the CLAUDE.md of this repo that..."*. A plain *"please remember"* causes Claude to store the information in internal memory files that can get lost.

## Habit 2: Skills — Every Tool Has Its Place

Skills are custom slash commands for Claude Code. A Markdown file in `.claude/commands/` that contains a prompt. When I type `/check-deploy`, the agent executes that prompt — the same way every time, no improvisation.

Instead of explaining the PDF solution path from scratch every time, I define it once as a skill. The learning: instruct Claude so that all available skills are listed and explained via a `/help` command.

**Proposal: `/org-help`** — a skill that lists all organization-specific slash commands with a short description. If you don't know what exists, you can't use it. Discovery isn't a nice-to-have — discovery is the prerequisite for adoption.

Skills live in `~/.claude/commands/` (global) or `.claude/commands/` (per repo). For a single project, that's fine. For a team or organization, distribution becomes the challenge — more on that shortly.

## Habit 3: MCP Servers — Call the Colleague from Another Trade

My experience so far:

**For an individual developer, MCP adds little value.** A bash script referenced in `CLAUDE.md` does functionally the same thing as an MCP tool. I can ask the agent to run a script — and it does. The protocol overhead of MCP isn't worth it for one person.

The real MCP advantage shows up for organizations and solves three problems:

**First: Clearly defined interfaces instead of local improvisation.** `CLAUDE.md` gives the agent an instruction: *"Use this script."* It can follow it — or improvise in a new session. An MCP tool gives the agent an interface: `generate_pdf` exists as a tool with defined inputs. It calls it or it doesn't — but it can't work around it. The difference: from *"the agent should do X"* to *"the agent can only do X"*. That's a different quality of consistency.

**Second: Central deployment instead of individual configuration.** An MCP server is a single deployment. New version, bug fix, new policy — change it once, all agents have it immediately. No onboarding script, no updating 200 machines.

**Third: Dynamic data instead of unclear contexts.** MCP can serve live context — current ticket status, infrastructure state, database queries. `CLAUDE.md` is static: it describes what was true when it was written.

**My verdict on MCP:** As an individual developer: skip it, `CLAUDE.md` and scripts are enough. For organizations beyond a certain size: MCP is the right long-term approach — not because it standardizes workflow, but because it enforces consistency and centralizes maintenance.

## Habit 4: The Morning Checklist

This is the piece that doesn't exist yet — and is the most urgently needed.

Imagine the craftsman has a morning checklist: *Power drill present? Drill charged? Level in the case?* Only when everything is checked off does he start work.

**Proposal: `/org-health`** — a skill that verifies whether the agent's setup is complete:
- Are all expected scripts present?
- Are all MCP servers reachable?
- Is the global `CLAUDE.md` up to date?

If something is missing, the agent fixes it — or clearly reports what needs manual action. The concept isn't new: CI/CD pipelines verify the state of code before every deployment. The agent needs the same for its own setup.

## What This Means for Organizations — and One Important Idea

Everything I've described so far works for an individual developer with some discipline. For an organization with 50, 200, or 1,000 developers, discipline is not a strategy.

What happens to me once happens 200 times in a 200-person org. Simultaneously, on different days, with different workarounds. Support gets flooded. Knowledge diverges. Teams reinvent wheels that others built months ago.

The question is: **What does "central" mean in an organization?**

**CLAUDE.md:** A canonical global template, versioned in an internal org repo. Changes via PR. Distribution via onboarding script or dotfiles management.

**Skills:** A central repo manages org-wide skills. An onboarding script symlinks them into `~/.claude/commands/`. Every developer gets the same slash commands — and `/org-help` shows what's available.

**MCP:** Instead of keeping scripts current on 200 machines, deploy an MCP server. All agents connect automatically. Updates are instant.

**Health check:** `/org-health` becomes first-level support. Before a developer opens a ticket, the agent checks its own setup. Most "the agent is acting weird" issues resolve through a missing MCP server or an outdated `CLAUDE.md`.

**Concretely: a `claude-setup` onboarding script**

```bash
# One-time on a new machine:
curl -s https://internal.org/claude-setup.sh | bash
```

The script copies the global `CLAUDE.md`, installs org skills, writes MCP server configuration, and runs `/org-health` as a final check. A new developer is ready in under a minute — with the same setup as everyone else.

## What Remains

AI agents aren't bad craftsmen. They're forgetful craftsmen. And the solution isn't to wait for better memory — the solution is to organize their tools so they can find them every morning.

For an individual developer, a `CLAUDE.md` and the discipline to write down working recipes is enough. For an organization, it's not. That requires a rigorous approach: versioned configuration, distributed skills, central MCP servers, and an agent that can verify its own state.

---

*How do you handle this? Have you experienced your agent "forgetting" something between sessions? I'd love to hear your take.*

## Changelog

**2026-04-04** — Removed status bar example, focused on PDF as the single running example. Replaced "layers" with "habits". Cuts and tightening.
