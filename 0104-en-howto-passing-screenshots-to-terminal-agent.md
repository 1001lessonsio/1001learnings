---
number: 104
lang: en
title: "Howto: My Workflow for Passing Screenshots to an Agent in the Terminal"
date: 2026-03-06
updated: 2026-03-09
stability: volatile
review_by: 2027-03-06
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# Howto: My Workflow for Passing Screenshots to an Agent in the Terminal

**A simple trick that completed my terminal workflow with Claude Code.**

2026-03-06 · Stefan Wendel · Claude Opus 4.6

---

> **Update 2026-03-09:** Claude Code now supports pasting images directly with `Ctrl+V` in the terminal — no file path needed. The workflow below is still useful: for structured sessions, multiple devices, and automatic pickup by the agent. Both approaches complement each other.

> This howto describes my workflow on **macOS**. The core idea works anywhere, but the specific steps use macOS built-in tools.

Since early 2026, I've been working almost exclusively in the terminal with my AI agents — running Claude Code in Ghostty. Before that, I was convinced I'd never leave my IDE. Now the terminal is my main tool. One thing was missing early on: **How do I show the agent what I see on my screen?**

In a terminal, there's no drag-and-drop. Claude Code now supports `Ctrl+V` for pasting images directly from the clipboard — handy for quick one-off cases. But for a structured, repeatable workflow, a fixed folder location is worth it: give your AI agent the path to your screenshots.

## The Workflow in Three Steps

**1. Set up a screenshot folder**

Create a dedicated folder for screenshots and configure macOS to save them there:

```bash
mkdir -p ~/Documents/_screenshots
defaults write com.apple.screencapture location ~/Documents/_screenshots
killall SystemUIServer
```

Tip 1: Don't use the desktop. You'll be taking lots of screenshots during your AI agent sessions. If your desktop is already cluttered with screenshots without agents — time to change that.

Tip 2: Put the screenshot folder on iCloud Drive (e.g. `~/Documents/_screenshots`). If you work on multiple Macs, your screenshots will be available across all devices automatically.

**2. Take a screenshot**

`Cmd + Shift + 4` — select an area, done. The file is saved automatically to your designated folder.

**3. Tell the agent**

In the terminal, just say:

```
Please look at the latest screenshot.
```

The agent reads the most recent screenshot from the folder and can react to it — give UI feedback, spot errors, answer layout questions.

## Why This Works

Claude Code can read images when given a file path. The fixed location makes the workflow predictable: no searching, no copying, no explaining. In my `CLAUDE.md`, I've configured the screenshot folder and the rules: the agent always picks the most recent screenshot, asks if something doesn't match the context, and proactively requests a screenshot for visual tasks. That saves me the explanation every time.

This isn't a feature, it's a workaround. But it works reliably every single day.

---

## Changelog

**2026-03-09** — Claude Code now supports `Ctrl+V` for pasting images directly in the terminal. Disclaimer and introduction updated accordingly.
