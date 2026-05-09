# jumpstart.md

**A structured guide for starting a software project from scratch.**

Drop it in your repo, point your agent at it, and it will walk you through every decision before a line of code is written.

---

## What it is

`jumpstart.md` is a single file you copy into any new project. It contains a structured sequence of questions and decisions that your AI agent runs through with you — covering everything from project shape and audience to infrastructure, stack, database, integrations, documentation, and legal basics.

The goal: no more starting a project only to discover three weeks in that you picked the wrong database, forgot about auth, or have no idea how to deploy it.

---

## How to use it

1. Start a new project.
2. Copy `jumpstart.md` to your repo root.
3. Tell your agent: *"Read jumpstart.md and help me set up this project from scratch."*
4. Answer the questions. Your agent will do the rest.

At the end of the session, your agent writes an `AGENTS.md` file capturing every decision made — ready to be picked up in any future session without losing context.

---

## Contents

- **Phase 0 — Exploration** — brainstorming, project type, prior research, repo structure
- **Phase 1 — Base infrastructure** — Docker, CI/CD, hosting
- **Phase 2 — Architecture** — UX/UI brief, stack, database, integrations (auth, payments, email, storage, analytics...)
- **Phase 3 — Documentation** — what to write before, during, and after development
- **Phase 4 — Launch** — domain, testing, security, legal, costs, backups
- **Annex A — Agent principles** — five rules for your agent to work by

---

## Status

Work in progress. Phase 0 is complete. The rest is coming.

Feel free to contribute or give feedback through an an issue.

---

Built with ❤️ by [alarcia](https://github.com/alarcia)