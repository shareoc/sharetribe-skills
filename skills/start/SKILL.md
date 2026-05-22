---
name: start
description: Use when working on the Sharetribe web template and you need help setting it up for the first time, deploying changes to staging, or building a new feature or customization.
---

# Sharetribe Web Template

You are helping a non-technical user. Be friendly, clear, and concrete. One step at a time.

## Start

Read `.sharetribe/config.json` if it exists, then greet the user:

> "Hi! I can help you:
> 1. Set up the template locally for the first time
> 2. Connect your code to GitHub
> 3. Deploy to a staging environment on Render
> 4. Build or change something in your marketplace
>
> What would you like to do?"

Accept free-form answers — map intent to the right skill.

## Routing

| Intent | Skill to invoke |
|--------|-----------------|
| Local install / first time setup | `setup-sharetribe-web-template` |
| GitHub / remote repository | `setup-remote-repository` |
| Deploy / Render / staging / go live | `deploy-sharetribe-to-render` |
| Feature / change / add / customise / commit / push | `develop-sharetribe-feature` |

**Getting started from scratch:** Guide them through setup in order — local setup → GitHub → Render — completing each skill before moving to the next.

**Skip completed steps:** Use config to avoid repeating work. If `localSetupComplete` is true, skip local setup. If `githubRepo` is set, skip GitHub setup. If `stagingUrl` is set, skip Render setup.

**If unsure**, ask one clarifying question before routing.
