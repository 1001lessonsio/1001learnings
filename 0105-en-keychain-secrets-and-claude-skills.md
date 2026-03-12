---
number: 105
lang: en
title: "Claude Skills — A Netlify Deploy Check Example"
date: 2026-03-12
stability: volatile
review_by: 2027-03-12
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# Claude Skills — A Netlify Deploy Check Example

**Stop reinventing the wheel — recurring agent workflows belong in a skill.**

2026-03-12 · Stefan Wendel · Claude Opus 4.6

---

A [skill](https://docs.anthropic.com/en/docs/claude-code/slash-commands) is a reusable slash command for Claude Code — a Markdown file with instructions that the agent executes on demand. Instead of typing the same prompt over and over, you define it once as a skill and invoke it with `/skill-name`.

Here's the workflow we want to turn into a skill: After merging a PR to `main`, Netlify triggers a build automatically. The question is always the same: Is the deploy done yet? Until now, I'd open the Netlify dashboard, dig through shell history for a curl command, or hit CMD-R 5 to 10 times. All of it breaks the flow. And flow is the new gold, so: never break the flow ;-)

The solution has two parts: Store the API token for the Netlify deploy status check securely on your machine — and package the entire workflow as a Claude Code skill, so it's available anytime with a single slash command.

## Part 1: Secrets in the macOS Keychain

macOS has a built-in, encrypted secret store: the Keychain. No Homebrew package, no external tool — the `security` CLI ships with every Mac.

**Store a token** (one-time):

```bash
security add-generic-password -s "netlify-api-token" -a "1001lessons" -w "YOUR_TOKEN"
```

**Retrieve a token** (in scripts or prompts):

```bash
security find-generic-password -s "netlify-api-token" -a "1001lessons" -w
```

That's it. The token sits encrypted in the Keychain, protected by your login password or Touch ID. No risk of accidentally committing it. No `.env` file that needs to be in `.gitignore` and still ends up in the wrong repo eventually.

This pattern works for any API token, SSH passphrase, or other secret you need in the shell.

## Part 2: Claude Code Skills

Claude Code supports custom slash commands — called skills. A skill file is just a Markdown file in `.claude/commands/` that contains a prompt. When I type `/check-deploy`, Claude executes that prompt.

**Create the file:** `.claude/commands/check-deploy.md`

```markdown
Check the current Netlify deploy status for 1001lessons.io.

Read the Netlify API token from the macOS Keychain:
security find-generic-password -s "netlify-api-token" -a "1001lessons" -w

Call the Netlify Deploy API and report:
- State (building / ready / error)
- Timestamp
- Deploy duration
```

**Run it:** Just type `/check-deploy` in Claude Code. The agent reads the token from the Keychain, calls the API, and reports the status. No copying curl commands, no opening dashboards.

## Why This Is a Good Pattern

The combination of Keychain and skill solves two problems at once:

**Secrets stay secure.** The token never appears in a prompt or a file that could end up in a repo. The agent reads it at runtime from the Keychain — exactly like a shell script would.

**Workflows become reusable.** Instead of explaining the same API call every time, I define the workflow once as a skill. The agent has the context — it knows which project, which site, which API. I type a slash command and get the result.

The principle applies to any recurring workflow: checking CI status, querying DNS records, fetching Lighthouse scores. If you're typing the same prompt for the third time, it's worth making a skill.

## Step by Step

1. **Create a Netlify Personal Access Token:** netlify.com → User Settings → Applications
2. **Store the token in Keychain:** `security add-generic-password -s "netlify-api-token" -a "ACCOUNT" -w "TOKEN"`
3. **Find your Netlify Site ID:** Site Settings → Site details
4. **Create the skill:** `.claude/commands/check-deploy.md` with the prompt template
5. **Test it:** Type `/check-deploy` in Claude Code

## Goodie on Top: Automatic Deploy Notifications

It goes one step further. In my setup, I don't even need to call `/check-deploy` myself — Claude reports the deploy status automatically when the context calls for it.

The trick: My `CLAUDE.md` includes an instruction telling the agent to check whether the deploy went through after a PR merge or push to `main`. Claude knows the skill, knows the context — and runs the check on its own. No slash command needed.

There's nothing magical happening under the hood. Claude Code reads the `CLAUDE.md` at the start of each session and sees the registered skills. When the situation fits — a PR was just merged, posts were just pushed to the content repo — the agent runs the skill autonomously and reports back: "Deploy is ready, took 45 seconds."

That's the real advantage of skills over shell aliases or bookmarks: The agent can not only execute them on command, but also proactively — because it understands the context in which they're useful.
