---
name: setup-sharetribe-web-template
description: Use when a user wants to install and configure the Sharetribe Web Template for the first time in a local development environment, including cloning the repo, gathering credentials, and running the dev server.
user_invocable: true
---

# Setup Sharetribe Web Template

## Overview

Interactive guide to clone, configure, and start the Sharetribe Web Template locally. Walk the user through each stage in order, asking for credentials at the right moment.

## Process

### Step 0: Check Existing Config

Read `.sharetribe/config.json` if it exists.

If it exists and contains `localSetupComplete: true`:
> "It looks like you've already set up the Sharetribe Web Template locally. Do you want to run setup again, or are you looking to do something else (like set up a GitHub repo or deploy to Render)?"

If the user wants to re-run, continue. Otherwise stop and point them to the relevant skill.

If the file doesn't exist, continue with Step 1.

### Step 1: Prerequisites

Ask the user to confirm they have installed:
- **Git** — [git-scm.com](https://git-scm.com/downloads)
- **Node.js** — [nodejs.org](https://nodejs.org/)
- **Yarn** — `npm install -g yarn` if not installed

Ask: "Do you know if you have Git, Node.js, and Yarn installed?"

### Step 2: Clone and Install

Once prerequisites are confirmed, run:

```bash
git clone https://github.com/sharetribe/web-template.git
cd web-template/
yarn install
```

Tell the user this installs all dependencies and may take a minute.

### Step 3: Collect Credentials

Before running config, ask the user for each credential one at a time using AskUserQuestion. Explain where to find each one.

**Sharetribe Client ID**
> Found in Sharetribe Console under **Build → Advanced → Applications**
> Sign up free at console.sharetribe.com/new if you don't have an account.
> **Important:** Make sure you're in the **Dev environment** (top-left switcher in Console) before copying the ID — the Test and Dev environments have different credentials, and using the wrong one is a common reason things don't work after setup.

**Sharetribe Client Secret**
> Same location: Console → Build → Advanced → Applications (still in Dev environment)
> Note: you're only copying the Secret here — don't edit any settings on this page.

**Stripe Publishable Key (Sandbox)**
> Set up Stripe in Console first, then copy the Sandbox publishable key.
> Guide: [Set up Stripe for a custom marketplace](https://www.sharetribe.com/help/en/articles/set-up-stripe)

### Step 4: Run Config Script

Once the user has their credentials ready:

```bash
yarn run config
```

Tell the user this script will prompt for the three values they just collected:
1. `REACT_APP_SHARETRIBE_SDK_CLIENT_ID` → paste client ID
2. `SHARETRIBE_SDK_CLIENT_SECRET` → paste client secret
3. `REACT_APP_STRIPE_PUBLISHABLE_KEY` → paste Stripe publishable key

The script creates a `.env` file. The app won't start without it.

### Step 5: Start the Server

```bash
yarn run dev
```

This starts the dev server with hot module replacement. The app opens automatically at `http://localhost:3000`.

### Step 6: Write Config

Create the `.sharetribe/` directory if it doesn't exist, then write (or merge into) `.sharetribe/config.json`:

```json
{
  "localSetupComplete": true
}
```

If the file already exists, merge this value in — do not overwrite other fields.

## Quick Reference

| Command | Purpose |
|---------|---------|
| `yarn install` | Install dependencies |
| `yarn run config` | Set up `.env` with required credentials |
| `yarn run dev` | Start local dev server |

## Common Issues

| Problem | Fix |
|---------|-----|
| App won't start | `.env` missing — re-run `yarn run config` |
| Map not loading | Configure map provider in Sharetribe Console |
| Payment errors | Check Stripe keys match what's in Console |

## Summary Commands

```bash
git clone https://github.com/sharetribe/web-template.git
cd web-template/
yarn install
yarn run config
yarn run dev
```
