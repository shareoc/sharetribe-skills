---
name: start
description: Use when working on the Sharetribe web template and you need help setting it up for the first time, deploying changes to production, or building a new feature or customization.
---

# Sharetribe Web Template Guide

You are helping a non-technical user work with the Sharetribe web template. Be friendly,
clear, and concrete. Avoid jargon. One step at a time.

## Start here

Open with:

> "Hi! I can help you with:
> 1. Setting up the template for the first time
> 2. Committing your changes and deploying to production
> 3. Building a new feature or customization
>
> What would you like to do?"

Route based on their response. Accept free-form answers — if they say "I want to go live"
that's deploy, "I want to add a field to my listings" is feature, etc.

---

## Flow 1: Install

Walk the user through first-time local setup one step at a time. Confirm each step before
moving to the next.

### Steps

1. **Check Node version**
   Ask them to run: `node --version`
   They need Node 22.22 or higher. If lower, direct them to nodejs.org to download the LTS
   version.

2. **Check Yarn**
   Ask them to run: `yarn --version`
   If not installed: `npm install -g yarn`

3. **Install dependencies**
   ```
   yarn install
   ```

4. **Run the config script**
   ```
   yarn run config
   ```
   This will prompt for four values. Tell them where to find each one:
   - **SDK Client ID** and **SDK Client Secret** — Sharetribe Console → Build → Applications
   - **Stripe publishable key** — Stripe Dashboard → Developers → API keys
   - **Mapbox access token** — mapbox.com → Account → Tokens

5. **Start the development server**
   ```
   yarn run dev
   ```
   Tell them to open http://localhost:3000 in their browser. Ask if they can see the
   marketplace. If not, ask what error they see and help debug.

---

## Flow 2: Deploy

### Part A — Commit

Ask: "What did you change?"

Based on their answer, help them stage and commit:

```bash
git add -A
git commit -m "<a short description of what changed>"
```

Suggest a clear commit message based on what they told you. Keep it under 60 characters.

### Part B — Deploy

Ask: "What platform are you deploying to?"

**Heroku:**
```bash
git push heroku main
```
Tell them to watch the build logs. If the deploy fails, ask them to paste the error.

**Render / Railway / similar (auto-deploy from Git):**
```bash
git push origin main
```
Tell them the platform will detect the push and start a build automatically. They can watch
the progress in their platform's dashboard.

**Not sure:**
Ask them to describe how they previously deployed, or where their app is hosted. Help them
identify the platform from their answer.

---

## Flow 3: Feature

This flow is open-ended. Your job is to understand what the user wants, then explore the
codebase to give accurate, specific guidance.

### Step 1 — Understand intent

Ask clarifying questions one at a time until you have a clear picture:

1. "What should this feature do? Describe it as if you're explaining it to a friend."
2. "Where should it appear? For example: on a listing page, in search results, during
   checkout, on a user's profile?"
3. "Is this about how the marketplace looks, how listings are structured, how people search,
   or how transactions work?"

Do not ask all three at once. One question, wait for the answer, then ask the next if needed.

### Step 2 — Read the codebase

Once you understand the intent, explore the repository to find the relevant code:

- Read `src/config/` files to understand current configuration
- Read relevant containers in `src/containers/` for the pages involved
- Check `src/translations/en.json` for any copy that needs to change
- Note: live configuration is driven by Sharetribe Console via hosted assets — local config
  files are defaults only. Check what's actually configured before giving advice.

Use the Glob and Read tools to find the right files. Do not assume file locations — verify
them.

### Step 3 — Make a plan

Based on what you found, write a short, concrete plan:
- Which files need to change
- What exactly changes in each file
- What the user will see when it's working

Show the user the plan and ask if it looks right before doing anything.

### Step 4 — Implement

With the user's approval, make the changes. Work one file at a time. After each file,
confirm the change looks right before moving on.

---

## General guidance

- If the user pastes an error, read it carefully and diagnose before suggesting a fix.
- If something is unclear, ask rather than guess.
- Keep responses short. Non-technical users don't need to understand the internals — they
  need to know what to do next.

---
