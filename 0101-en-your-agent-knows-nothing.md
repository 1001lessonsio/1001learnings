---
number: 101
lang: en
title: "Your Agent Knows Nothing. Shift Into — or It Never Will."
date: 2026-02-28
stability: evergreen
review_by: 2028-02-28
author: "Stefan Wendel"
coauthor: "Claude Sonnet 4.6"
coauthor_id: claude-sonnet-4-6
---

# Your Agent Knows Nothing. Shift Into — or It Never Will.

**Shift documentation into your build — or ship hallucinations.**

2026-02-28 · Stefan Wendel · Claude Sonnet 4.6

---

We are in the middle of a paradigm shift. AI agents write code, refactor modules, generate tests, create pull requests. Tools like Cursor, Claude Code, Copilot Workspace, or Devin promise a future where we no longer type software line by line, but steer it at a higher level. Sounds great. And it is — with one massive catch.

**The agent you just started is being born at this very moment.**

It has no memory. No implicit knowledge. No recollection of last week's meeting where you decided against GraphQL and went with REST instead. No understanding of why the payment service is built the way it is. Every prompt, every new chat is a reset to zero. The agent has to re-learn the world of your project in seconds — and the only material available to it is what you give it.

And that is exactly where documentation suddenly shifts from "nice to have" to "mission-critical".

## Garbage In, Garbage Out — Reloaded

In classical software development, poor documentation was annoying. The new colleague just needed two extra weeks to onboard. Someone asked at the watercooler, and somebody knew the answer.

In agent-assisted development, there is no watercooler. There is no colleague who "quickly explains it". There is only the context you provide to the agent. And that determines the quality of the output — deterministically and mercilessly.

**Documentation that is outdated** causes the agent to develop against an API version that no longer exists. Or to use patterns you abandoned long ago.

**Documentation that is missing** creates gaps. And gaps, as we know, get filled by an LLM creatively — with hallucinations. The agent then invents endpoints that don't exist, or makes architectural decisions that contradict your strategy.

**Documentation that is ambiguous** leads to wrong turns. The agent takes the most plausible path — and that isn't always the right one.

## Groundhog Day: The Agent Is Your New Team Member. Every Day. For the First Time.

Imagine having to explain your entire project every morning to a brilliant but completely amnesiac developer. Everything. From the architecture to business rules to your team's conventions. Every. Single. Day.

That is exactly what happens when you work with AI agents. And the quality of that "explanation" — i.e., your documentation — determines whether the agent is productive or causes chaos.

This means: we must understand documentation as a living, maintained system. Not as a box-ticking exercise that gets written once and then forgotten.

## What Concretely Needs to Be Done

**Everything in Git, versioned, close to the code.** Every architectural decision, every ADR (Architecture Decision Record), every API spec, every convention — all of it belongs in the repository. Not in a Confluence wiki that hasn't been touched since 2022. Not in the tech lead's head. In Git. With the code. Versioned. Reviewed. Current.

**Documentation must be kept up to date.** This sounds obvious, but it's the hardest part. A document that describes the state from six months ago is worse than no document at all — because it actively misleads. Every change to the system must trigger a question: Is the documentation still accurate?

**Gaps must be systematically identified.** What is not documented? What implicit knowledge exists only in the heads of team members? Where will an agent inevitably hit an information vacuum? Identifying these gaps is not a one-time task — it's an ongoing process.

**Missing knowledge must be actively filled in.** If you notice that an agent consistently takes a wrong turn at a particular point — document exactly that point. Every agent mistake is a signal for missing or unclear documentation.

## Docfooding: Agents Maintain the Docs They Need Themselves

The same agents that need good documentation can help create and maintain that documentation:

An agent can analyze your code and identify where documentation is missing or outdated. It can automatically generate summaries from code changes. It can propose ADRs in the correct format when architectural decisions are made. It can check existing docs against the current state of the code and flag inconsistencies.

This is not a contradiction — it's a positive feedback loop. Better documentation leads to better agent outputs, and agents help make the documentation better.

## The Bottom Line

In a world where AI agents are increasingly becoming real team members, documentation is no longer a bureaucratic chore. It is the interface between human intention and machine execution. It is the context that determines whether your agent is a valuable colleague or an uncontrollable hallucination machine.

> **Shift Into: Documentation is now part of the build process.**
>
> **Not Shift Left, not Shift Right — Shift Into. Documentation is not a process step you can move around. Documentation is now an integral part of the build itself. Like the compile step. Like the verify step. Shift documentation into your build — or ship hallucinations.**

The teams that understand this early and take their documentation-as-code practice seriously will be productive with AI agents. The others will wonder why "AI-assisted development" doesn't work for them.

The answer usually isn't in the tool. It's in your docs.

---

*How do you keep your documentation up to date? Have you had experiences with agents that failed because of poor docs? I'd love to exchange thoughts.*
