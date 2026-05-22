# Sharetribe Skills

Claude skills for non-technical users working on the Sharetribe web template.

## What this does

Gives you a guided assistant inside Claude Code. Type `/sharetribe:guide` and Claude
will help you with:

- Setting up the template for the first time
- Committing your changes and deploying to production
- Building a new feature or customization

## One-time setup

**1. Clone this repo into your Sharetribe project folder:**

```bash
git clone https://github.com/shareoc/sharetribe-skills.git
```

**2. Open Claude Code in your Sharetribe project, then run:**

```
/plugin install ./sharetribe-skills --scope project
```

That's it. The skill is now available every time you open Claude Code in this project.

## Usage

```
/sharetribe:start
```

Claude will ask what you need and walk you through it.
