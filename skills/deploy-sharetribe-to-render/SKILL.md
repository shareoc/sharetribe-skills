---
name: deploy-sharetribe-to-render
description: Use when a user wants to deploy the Sharetribe Web Template to Render as a staging environment, including writing render.yaml, configuring environment variables, and walking through the Render dashboard setup.
user_invocable: true
---

# Deploy Sharetribe Web Template to Render

## Overview

Interactive guide to deploy the Sharetribe Web Template to Render as a staging environment. The skill writes `render.yaml` to the repo (pre-filling all non-secret values), then walks the user through each dashboard step with checkpoints.

## Config File

This skill reads and writes `.sharetribe/config.json` to share state with other Sharetribe skills.

## Process

### Step 0: Check Existing Config

Read `.sharetribe/config.json` if it exists.

If it contains `renderServiceName` and `stagingUrl`:
> "It looks like you've already set up a Render deployment: `<stagingUrl>`. Do you want to configure a new deployment, or update the existing one?"

If the user wants to continue anyway, pre-fill the service name and marketplace name from config where available and ask the user to confirm or change each value.

If no config or no `renderServiceName`, continue with Step 1.

### Step 1: Collect Setup Info

Ask the user two questions before touching any files:

**1a. Service name**
> "What do you want to name your Render service? This becomes your URL: `https://<name>.onrender.com`"

If `renderServiceName` was found in config, suggest it as the default.

Also ask for:
- **Marketplace name** (e.g. "My Marketplace") — pre-fill from config `marketplaceName` if present

**1b. Basic auth**
> "Do you want to password-protect your staging environment? This is recommended — it keeps the site private while you're still developing and prevents search engine indexing."

If yes: ask for a username and password they want to use.

Note the `REACT_APP_ENV` value depends on this:
- Basic auth enabled → `REACT_APP_ENV=production`
- Basic auth disabled → `REACT_APP_ENV=development`

### Step 2: Write render.yaml

Write the following file to the root of the repo, substituting `<service-name>` and `<REACT_APP_ENV_VALUE>` from Step 1:

```yaml
services:
  - type: web
    name: <service-name>
    runtime: node
    buildCommand: yarn install --production=false; yarn build
    envVars:
      - key: NODE_VERSION
        value: 22.22.0
      - key: NODE_ENV
        value: production
      - key: REACT_APP_ENV
        value: <REACT_APP_ENV_VALUE>
      - key: REACT_APP_MARKETPLACE_ROOT_URL
        value: https://<service-name>.onrender.com
      - key: REACT_APP_SHARETRIBE_USING_SSL
        value: false
      - key: SERVER_SHARETRIBE_TRUST_PROXY
        value: true
      - key: REACT_APP_CSP
        value: block
      - key: REACT_APP_AVAILABILITY_ENABLED
        value: true
      - key: REACT_APP_DEFAULT_SEARCHES_ENABLED
        value: true
      - key: REACT_APP_SHARETRIBE_SDK_CLIENT_ID
        sync: false
      - key: SHARETRIBE_SDK_CLIENT_SECRET
        sync: false
      - key: REACT_APP_STRIPE_PUBLISHABLE_KEY
        sync: false
      - key: REACT_APP_MAPBOX_ACCESS_TOKEN
        sync: false
      - key: REACT_APP_MARKETPLACE_NAME
        sync: false
```

If basic auth was requested, append to `envVars`:
```yaml
      - key: BASIC_AUTH_USERNAME
        sync: false
      - key: BASIC_AUTH_PASSWORD
        sync: false
```

Tell the user the file has been written and explain that `sync: false` means Render will prompt them to fill those values in the dashboard.

### Step 3: Test SSR Locally

Before deploying, ask the user to test server-side rendering locally:

```bash
yarn run dev-server
```

> "Does the app load without errors at http://localhost:4000?"

If they hit errors mentioning `window` or browser globals — explain these are SSR errors caused by code that references browser APIs. They need to fix these before deploying or the production server will crash.

Only proceed once SSR is confirmed working.

### Step 4: Commit and Push

Ask the user to commit and push `render.yaml`:

```bash
git add render.yaml
git commit -m "Add Render deployment config"
git push
```

Ask: "Done? Is render.yaml pushed to your GitHub repository?"

### Step 5: Render Dashboard Walkthrough

Walk through each step, asking "Done?" before moving to the next.

**5a. Create account**
> "Do you have a Render account? If not, sign up at https://dashboard.render.com/register"

**5b. Create new Web Service**
> "In the Render dashboard, click New → Web Service, then connect your GitHub account and select your repository."

**5c. Configure build command**
> "In the service settings, find the Build Command field and set it to:
> `yarn install --production=false; yarn build`"

**5d. Add NODE_VERSION**
> "Before clicking Create, click Advanced and add this environment variable:
> - Key: `NODE_VERSION`  Value: `22.22.0`"

**5e. Add secret environment variables**
Tell the user to add each of the following in the Advanced section. For each one, tell them where to find the value:

| Variable | Value / Where to find it |
|----------|--------------------------|
| `REACT_APP_SHARETRIBE_SDK_CLIENT_ID` | Sharetribe Console → Build → Advanced → Applications. Use your **Dev environment** client ID for a dev staging app. |
| `SHARETRIBE_SDK_CLIENT_SECRET` | Same location as client ID |
| `REACT_APP_STRIPE_PUBLISHABLE_KEY` | Stripe dashboard — use the test key starting with `pk_test` |
| `REACT_APP_MAPBOX_ACCESS_TOKEN` | Mapbox account dashboard (if using Mapbox) |
| `REACT_APP_MARKETPLACE_NAME` | Whatever you want your marketplace to be called |

If basic auth was enabled, also add:
| `BASIC_AUTH_USERNAME` | The username chosen in Step 1 |
| `BASIC_AUTH_PASSWORD` | The password chosen in Step 1 |

**5f. Create web service**
> "Click 'Create Web Service'. The first build takes a few minutes — you can watch the logs in the Render dashboard."

### Step 6: Write Config

Write (or merge into) `.sharetribe/config.json`:

```json
{
  "renderServiceName": "<service-name>",
  "stagingUrl": "https://<service-name>.onrender.com",
  "basicAuthEnabled": true,
  "marketplaceName": "<marketplace-name>",
}
```

Set `basicAuthEnabled` to `false` if the user chose not to use basic auth.
If the file already exists, merge these fields — do not overwrite other fields (e.g. `githubRepo`).

### Step 7: Verify

Once the build completes:

> "Visit https://<service-name>.onrender.com — does your marketplace load?"

If basic auth is enabled, remind the user to test that the password prompt appears.

## Common Issues

| Problem | Fix |
|---------|-----|
| Build fails with yarn error | Check build command is exactly `yarn install --production=false; yarn build` |
| White screen / server crash | SSR error — check logs for `window is not defined`. Fix browser-only code before redeploying. |
| Marketplace shows wrong name | Set `REACT_APP_MARKETPLACE_NAME` env var and redeploy |
| Map not loading | Add `REACT_APP_MAPBOX_ACCESS_TOKEN` and redeploy |
| Basic auth not appearing | Check `REACT_APP_ENV=production` is set |
| Changes not appearing | Env vars with `REACT_APP_` prefix are baked into the build — redeploy after any change |
