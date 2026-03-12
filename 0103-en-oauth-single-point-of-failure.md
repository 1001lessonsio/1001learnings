---
number: 103
lang: en
title: "\"Login with Google\" on GitHub Is a Bad Practice"
date: 2026-03-04
stability: stable
review_by: 2028-03-04
author: "Stefan Wendel"
coauthor: "Claude Opus 4.6"
coauthor_id: claude-opus-4-6
---

# "Login with Google" on GitHub Is a Bad Practice

**Your OAuth provider is a single point of failure. Treat it like one.**

2026-03-04 · Stefan Wendel · Claude Opus 4.6

---

"Login with Google" is convenient. One click, no password, no friction. Most developers use it without a second thought — on GitHub, Vercel, Netlify, everything. And that's exactly the problem.

If you log into GitHub exclusively via an OAuth provider like Google, you're handing over control of your account. Not to GitHub — to Google. And Google can cut that access at any time without GitHub even knowing.

## What Happens When Google Locks You Out

Google suspends accounts. This isn't an edge case — it happens. The reasons are varied: multiple accounts linked to the same phone number, suspected policy violations, algorithmic false positives. In my case — the reason for this post — I had multiple Gmail accounts, one per project, all linked to the same phone number. Understandable that Google flags this as suspicious. But the consequences are critical.

The appeals process takes time. In my case, two days until a clear and positive resolution. Fair enough. But two days of total lockout across all affected services — and it could have gone much worse.

If your Google account gets suspended and you use GitHub exclusively through "Login with Google," here's what happens:

- **GitHub login fails.** No error dialog, no warning — the OAuth flow redirects you to Google, Google blocks you, and all GitHub sees is: authentication failed.
- **Every service tied to the same OAuth provider goes down simultaneously.** Vercel, Netlify, Railway, Supabase — everything where you used "Login with Google" is gone in one stroke.
- **Your repos aren't deleted**, but you can't reach them. No push, no pull, no access to issues or settings.

For a hobby project, that's annoying. For a commercial project, it's an unacceptable single point of failure.

## The Fix

The solution makes you independent of your OAuth provider:

### 1. Set an Email and Password Directly on GitHub

Go to your GitHub settings and make sure you have an email address and password set — regardless of whether you normally log in via Google. This is your fallback if the OAuth provider goes down.

### 2. Keep OAuth as an Additional Login Option Only

"Login with Google" as a convenience feature is fine. As your only way in, it's negligent. The question you should ask yourself: *Can I still log in if Google suspends my account tomorrow?*

### 3. Enable 2FA with an Authenticator App

Use an authenticator app like 1Password or Authy — not SMS. SIM swapping is a real attack vector where attackers transfer your phone number to a new SIM and bypass SMS-based 2FA.

### 4. Store Recovery Codes Securely

GitHub generates recovery codes when you set up 2FA. These codes are your last lifeline when both your OAuth provider and your authenticator app are unavailable. Store them in your password manager — not in a text file on your desktop.

### 5. Use a Dedicated Email for Your Asset

Treat every project as a standalone asset. Imagine you wanted to hand it off — a sale, a transfer to a partner, onboarding a new maintainer. How easy is that when every service is tied to your personal Gmail address?

Use an email address for GitHub and other development-critical services that isn't tied to your primary Google account. A custom domain with catch-all — a configuration where any address under your domain is accepted (`github@yourdomain.com`, `vercel@yourdomain.com`, etc.) — is the most professional solution here.

## The Pattern Behind This

The problem isn't Google. The problem is the implicit assumption that an external service will always be available. OAuth login delegates authentication to a third party — and with it, the risk. If you don't actively mitigate this, you're building a single point of failure into your entire development infrastructure.

This doesn't just apply to Google and GitHub. It applies to every combination of OAuth provider and critical service. Apple ID and iCloud. Microsoft account and Azure. The logic is always the same: **Never have only one way in.**

> **OAuth is a convenience feature, not a security architecture. Treat it like one.**

---

*Have you ever lost access to a service because your OAuth provider was unreachable?*
