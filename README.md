# Sharetribe Skills

Claude skills for non-technical users working on the Sharetribe web template.

## What this does

Gives you a guided assistant inside Claude Code. Type `/sharetribe:guide` and Claude
will help you with:

- Setting up the template for the first time
- Committing your changes and deploying to production
- Building a new feature or customization

## One-time setup

Open Claude Code in this project directory and run:

```
/plugin install ./sharetribe-skills --scope project
```

That's it. The skill is now available every time you open Claude Code in this project.

## Usage

```
/sharetribe:guide
```

Claude will ask what you need and walk you through it.
