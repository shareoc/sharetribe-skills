---
name: develop-sharetribe-feature
description: Use when a user wants to add, change, or remove something in their Sharetribe marketplace — a feature, visual change, content update, or behaviour. Guides requirements gathering before any implementation, then commits and pushes to staging.
user_invocable: true
---

# Develop a Sharetribe Feature

## Overview

Guides the user from a vague idea to a deployed change. Always gather requirements first — ask questions until you understand exactly what needs to be built. Then implement, commit, and push. The user never needs to think about git.

## Process

### Step 0: Read Config

Read `.sharetribe/config.json` if it exists. Note:
- `stagingUrl` — where the user will see their change
- `marketplaceName` — useful context for understanding the product

If config doesn't exist, continue without it (but you'll have less context).

### Step 1: Understand the Request

The user has described something they want to change or add. Before touching any code, ask clarifying questions **one at a time** until you understand:

- **What** exactly should change (text, layout, behaviour, new section, etc.)
- **Where** in the app it should appear (which page, which component)
- **How** it should look or behave (any specific content, colours, interactions)
- **Why** — understanding the goal helps you make better implementation decisions

**Rules for this phase:**
- One question per message. Never bundle questions.
- Prefer concrete options over open-ended questions where possible. ("Do you want this to appear at the top of the page, or at the bottom?")
- If the request involves text content, ask the user to provide the exact wording.
- Keep your language plain — avoid technical terms like "component", "prop", "state".
- If the user gives a vague answer, ask a more specific follow-up rather than guessing.

Continue asking until you are confident you understand what to build. Do not proceed to Step 2 until then.

### Step 1B: Check If This Can Be Done Without Code

Before writing any code, assess whether the request can be handled entirely through the Sharetribe Console. This saves deployment time and keeps the codebase simpler.

**No-code is possible for:**

| Category | What you can change |
|----------|---------------------|
| **Branding** | Primary color, logo, favicon, app icon, login background image, social media image |
| **Content pages** | Landing page (hero, sections, CTAs), Terms of Service, Privacy Policy, About page, custom content pages. If the request relates to either CREATING or MODIFYING a content page, load `guides/content-pages.md` to understand what can be achieved using no-code. |
| **Marketplace texts** | Button labels, error messages, help text, any UI copy across the site |
| **Email texts** | Content of automatic emails (booking confirmations, quote requests, offers, etc.) |
| **Footer** | Links, social media profiles, copyright text |
| **SEO & social meta** | Page title, description, social sharing image (per page) |
| **Layout** | Configure search page layout, listing page layout, listing thumbnail aspect ratio |
| **Top bar** | Show/hide search bar, show/hide "Post a listing" button, custom links, logo link target |
| **Listing types** | Choose one of four pre-defined transaction modesl (booking, product, negotiaton or free-inquiry) and select booking unit (daily/nightly/hourly) |
| **Listing fields** | Custom fields (dropdowns, multi-select, long text, etc.), display on listing page, search filters |
| **User fields** | Custom signup/profile fields, restricted to specific user types (e.g. providers only) |
| **Listing search** | Keyword vs location search, which filters are enabled |
| **Commission & monetization** | Marketplace commission percentage, transaction minimums |
| **Access control** | Who can publish listings (e.g. verified providers only) |
| **Integrations & automations** | Enter the API key to your Stripe, Google Analytics, or Map provider. You can also use Zapier to create certain no-code automations. |

**If the request is fully achievable via Console:**

Tell the user and guide them to the right place — do not write any code:

> "Good news — you don't need any code changes for this. You can do it directly in your Sharetribe Console under [specific section]. Here's how: [step-by-step instructions]"

Confirm they've made the change, then the conversation is done. Do not proceed to Step 2.

**If it's a mix (partly Console, partly code):**

Handle the Console part first, then continue to Step 2 for the rest:

> "Part of this can be done without code — let's start there. Then I'll make the remaining code changes."

**If it requires code:** Continue to Step 2.

### Step 2: Confirm Understanding

Before writing any code, summarise what you understood in plain, friendly language:

> "Just to make sure I've got this right — here's what I'm planning to do:
> [clear, non-technical description of the change]
> Does that sound right?"

If the user corrects you, go back and ask follow-up questions. Only proceed once they confirm.

### Step 3: Implement

Now make the code changes. Use your knowledge of the Sharetribe Web Template codebase to find the right files.

Guidelines:
- Make the smallest change that achieves what the user asked for
- Follow existing code style and patterns => the existing patterns are there for a reason.
- Do not refactor or "improve" surrounding code
- Do not add features beyond what was confirmed in Step 2

### Step 4: Review Locally

Ask the user to check the change on their local dev server (`http://localhost:3000`):

> "I've made the change. If your local server is running (`yarn run dev`), you can check it at http://localhost:3000. Does it look right?"

If the user spots something off, ask them to describe it specifically and fix it. Repeat until they're happy.

If they don't have the dev server running, offer to help them start it, or skip ahead if they're comfortable going straight to staging.

### Step 5: Commit and Push

Once the user is happy, commit and push without explaining git details:

```bash
git add -A
git commit -m "<descriptive message summarising the change in plain language>"
git push origin main
```

Write the commit message in plain language describing the change from a user perspective, not a developer perspective.

Examples:
- "Add contact us section to the homepage"
- "Change the hero banner text to welcome message"
- "Remove the featured listings section from the landing page"

Tell the user simply:
> "Done! Your change is on its way to staging. Render takes about 2–4 minutes to rebuild — you can check it at `<stagingUrl>` once it's ready."

If `stagingUrl` is not in config, tell them to check their Render dashboard for the URL.

### Step 6: Verify on Staging

After a few minutes, ask:

> "Does the change look good on staging at `<stagingUrl>`?"

If something looks wrong on staging but was fine locally, it may be an SSR (server-side rendering) issue. Symptoms: page crashes or shows a blank screen. If this happens:
- Check the Render logs
- Look for errors mentioning `window`, `document`, or browser-only APIs
- Fix the issue and push again (repeat from Step 5)

## Tone

The user may be non-technical. Use plain language throughout:
- Say "page" not "route"
- Say "the part of the page that shows listings" not "the ListingCard component"
- Say "I'll make that change" not "I'll update the JSX"
- When something goes wrong, explain what happened simply and what you're going to do about it

## Common Issues

| Problem | Fix |
|---------|-----|
| Change not visible on staging | Wait 2–4 min for Render to rebuild, then hard-refresh the browser |
| Works locally but crashes on staging | SSR error — look for browser-only APIs used outside of `useEffect` |
| Push rejected | Run `git pull origin main` first, then push again |
| Wrong file edited | Read the surrounding code more carefully; ask user for a screenshot if unsure where something lives |
