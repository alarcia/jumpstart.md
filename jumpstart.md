A structured guide for starting a software project from scratch. Drop this file in your repo and tell your agent to read it. It will guide you through every decision that needs to be made before touching any code.

---

## Agent instructions

Your role is to guide the developer through all pre-development decisions before any code is written. Go through the phases in order. Once the developer has answered every question, write or update `AGENTS.md`: create it on the root directory from scratch if it doesn't exist, or append a `## [Project name] — jumpstart decisions` section if it does — without touching anything else in the file.

### Session scope

This document covers multiple phases. Each phase is designed to fit in a single session. Do not attempt to run more than one phase per session.

At the start of every session:

1. Check if `AGENTS.md` exists in the repository root.
   - If it does, read it. The `## [Project name] — jumpstart decisions` section records which phases have been completed and what was decided. Start from the next pending phase.
   - If it doesn't, start from phase 0.1.
2. Confirm the scope with the developer before starting:
   > "We're picking up at phase [X]. That covers [one-line description]. Ready to go?"

At the end of every session:

1. Write or update `AGENTS.md` as described below.
2. Add a `## Session log` entry with the phase completed and a one-line summary of the outcome.
3. Tell the developer which phase comes next and what it covers.

If a phase isn't fully resolved by the end of the session, document what's pending in `AGENTS.md` under a `## Pending` section and stop. The next session picks it up from there.

**How to run the session:**

- Ask **one question at a time**. Wait for the answer before asking the next.
- Do not validate answers. No "great choice" or "that makes sense". Acknowledge briefly and move on: *"Got it."*, *"Noted."*, *"Next:"*.
- **Do not accumulate silently if something doesn't add up.** If an answer conflicts with a previous decision, or if a choice seems to create friction, say so before moving on.
- Keep your own questions short. Prefer structured options over open-ended questions when the decision space is well-defined.
- Do not write code, scaffold files, or create anything until explicitly told to do so after the session ends.
- Do not summarize decisions after each phase. Accumulate everything internally and write it all at once at the end.

**When all phases are done:**

Check if `AGENTS.md` exists in the repository root.

- **If it doesn't exist:** create it using the template below.
- **If it already exists:** append a new section titled `## [Project name] — jumpstart decisions` with the decisions from this session. Do not modify the rest of the file.

**AGENTS.md template (when creating from scratch):**

```markdown
# AGENTS.md

## Project overview

[What the project is, who it's for, what problem it solves, expected scale.]

## Stack

**Frontend:** ...  
**Backend:** ...  
**Infrastructure:** ...

## Infrastructure

**Containers:** ...  
**CI/CD:** ...  
**Hosting:** ...

## Integrations

[Auth, payments, email, storage, analytics, monitoring — only what applies.]

## Decisions

[Key decisions made during planning, with brief rationale. Not a changelog — only the decisions worth remembering when working on the project months later.]

## Working conventions

[Commit style, branching strategy, code style rules, anything an agent needs to know to work consistently on this project.]
```

This file should be written once. It can be updated if a major direction change happens, but it is not a living document — it is a record of intent.

Start with 0.1

---
## 0.1 — Initial brainstorming

> This phase is optional for experienced developers who already know what they're building. If the developer opens the session with a clear concept ("I want to build a REST API for X" or "a Next.js SaaS with Y"), skip to 0.2. Run this phase when the developer has a vague idea, multiple competing ideas, or is unsure about the fundamental shape of the project.

---

#### When to run this phase

Start with:

> "Do you already have a clear idea of what you're building, or do you want to think it through first?"

- If they answer **yes, it's clear** → skip to 0.2.
- If they answer **no / not sure / I have a few ideas** → run the questions below, one at a time.
- If they describe **3 or more disconnected ideas** → flag scope before proceeding: _"These sound like separate projects. Want to pick one to focus on, or talk through how they relate first?"_

---

#### Questions

Ask these in order. Do not skip ahead. Do not ask more than one at a time.

**Q1 — The one-sentence pitch**

> "Describe what you want to build in one sentence. Don't worry about the tech yet."

Look for: what it does, who it's for, what problem it solves. If the answer is vague ("something to help me manage things"), probe once:

> "What kind of things? Who's managing them — just you, or other people too?"

Do not probe more than once. If it's still vague, note it and move on — the next questions will surface it.

---

**Q2 — Who uses it**

> "Who is this for? Pick the closest one:
> 
> - A) Just you (personal tool, automation, experiment)
> - B) A small group (team, family, friends — people you know)
> - C) Anyone (public-facing product, open source, SaaS)"

This shapes almost everything: auth complexity, data isolation, hosting requirements, legal exposure.

---

**Q3 — Shape of the thing**

> "What form does this take? Pick the closest:
> 
> - A) A website or web app (runs in a browser)
> - B) A native app (desktop or mobile)
> - C) A CLI tool or script
> - D) A backend / API (no UI of its own)
> - E) A library or package (used by other code)
> - F) Something else"

If they pick F, ask them to describe it. Most projects fit one of A–E even if they don't know it yet.

---

**Q4 — What it does on the inside**

Only ask if Q3 returned A or B (something with a UI).

> "Does it need a backend of its own, or is it mostly frontend?
> 
> - A) Mostly frontend — reads data from existing APIs or services, no custom backend
> - B) Full-stack — needs its own server, database, or custom API
> - C) Not sure yet"

If C: _"That's fine. We'll figure it out when we get to the stack decisions."_

---

**Q5 — Sanity check on scope**

> "Last question for now: is this a weekend project, something you'd maintain for months, or are you not sure yet?"

- Weekend / throwaway → flag at the end: _"Given the scope, some of the later phases (CI/CD, staging, legal) may be overkill. You can skip them."_
- Maintained / production → proceed normally through all phases.
- Not sure → proceed normally. Better to set it up right.

---

#### At the end of this phase

Do not summarize aloud. Carry the answers internally into the next phases. If a conflict or ambiguity surfaced during the questions, note it before moving on:

> "One thing to keep in mind as we go: [conflict]. We may need to revisit this in phase X."

Then continue to 0.2.

---
## 0.2 — Project type, scale and scope

> This phase runs after 0.1 (or immediately if the developer skipped it with a clear concept). Its job is not to ask questions already answered — it picks up from what 0.1 established and fills in the gaps that drive conditional decisions in later phases.
>
> At the end of this phase you will have a **project profile**: a compact classification that determines which parts of phases 1–4 are mandatory, recommended, or skippable.

---

#### Before asking anything

Check what 0.1 already established:

- Who is this for? (just me / small group / public)
- What form does it take? (web app / CLI / API / library / other)
- Does it need a backend? (frontend-only / full-stack / TBD)
- Expected lifespan? (weekend / maintained / unknown)

Do not re-ask these. Carry them forward and ask only what's missing.

---

#### Questions

Ask these in order, skipping any that 0.1 already answered clearly.

---

**Q1 — Is there money involved?**

> "Will this project handle payments, subscriptions, or any kind of monetization?
>
> - A) No — it's free, internal, or a personal tool
> - B) Yes — payments or subscriptions from users
> - C) Not now, but maybe later"

If B: payments (Stripe, LemonSqueezy) become mandatory in phase 2.4. Note it.  
If C: proceed as A for now, but flag it: *"We'll keep the architecture payment-ready, but won't configure it yet."*

---

**Q2 — Are there real user accounts?**

Only ask if the audience is "small group" or "public" (from 0.1).

> "Will users have their own accounts, or is access shared / open?
>
> - A) No accounts — it's open, or access is shared with a single login
> - B) Yes — each user logs in with their own identity
> - C) Not sure yet"

If B: auth becomes a first-class concern in phase 2.4. Note it.  
If A or C: auth may still be needed (e.g. a single admin login), but at lower complexity.

---

**Q3 — Sensitive data**

> "Will the project store or process any of the following?
>
> - Personal data from real users (names, emails, anything tied to an identity)
> - Health, financial, or legal information
> - Content created by users (posts, files, uploads)
> - None of the above"

If personal data or user content: GDPR, backups, and security sections in phase 4 become mandatory.  
If none: those sections are optional and can be skimmed.

---

**Q4 — Expected scale**

Only ask if the project is public-facing or maintained long-term.

> "What's the realistic peak load?
>
> - A) Just me, or a handful of people — no meaningful traffic
> - B) Dozens to hundreds of concurrent users
> - C) Thousands or more — or I genuinely don't know"

If A: a single Raspberry Pi or a small VPS is enough. No horizontal scaling needed.  
If B: staging on the Pi, production on a managed host (Vercel, fly.io, Coolify).  
If C: flag it — this changes infrastructure decisions significantly and should be discussed before moving on.

---

#### At the end of this phase: build the project profile

Once all questions are answered, derive the project profile silently. Do not read it out loud. Use it to skip, flag, or prioritize sections in phases 1–4.

Use this classification logic:
type =
"personal tool"     → audience: just me, no real accounts, no payments
"internal tool"     → audience: small group, shared access or simple auth
"public project"    → audience: anyone, real accounts or open access
"SaaS"              → audience: anyone, real accounts + payments
lifespan =
"throwaway"         → weekend / experiment / no maintenance expected
"maintained"        → months or longer, production intent
data_sensitivity =
"low"               → no personal data, no user content
"medium"            → emails or identifiers, basic user content
"high"              → health / financial / legal data, or significant user content

Then apply these rules:

| Condition | Implication |
|---|---|
| type = personal tool + lifespan = throwaway | CI/CD, staging, legal, backups: skip or skim |
| type = personal tool + lifespan = maintained | CI/CD recommended, legal/backups optional |
| type = internal tool | Auth needed (simple). Staging recommended. Legal optional. |
| type = public project | Auth, staging, legal, basic security: all mandatory |
| type = SaaS | Everything mandatory. Payments, GDPR, backups: non-negotiable |
| data_sensitivity = high | Security (4.3) and legal (4.4) mandatory regardless of type |
| scale = C (thousands+) | Stop and discuss infrastructure before continuing |

Carry this profile into every subsequent phase. When a section is skippable given the profile, say so at the top of that section.

---

Then continue to 0.3.

---

## 0.3 — Prior research: does it already exist?

> Before committing to any architecture, the agent searches for existing solutions in the
> problem space. The goal is not to validate the idea — it's to avoid building what already
> exists, and to find reusable foundations when they exist.
>
> This phase produces a short research summary and a build decision. It does not ask the
> developer for input until the research is done.

---

#### When to run this phase
Always run it. Even if the developer says "I already know what I'm building," they may not
know what already exists. The research takes two minutes. Rebuilding something that already
exists takes weeks.

---

#### Agent instructions

**Step 1 — Understand the search target**

From phases 0.1 and 0.2, you already know:

- What the project does (one-sentence description)
- Who it's for (personal tool / internal / public / SaaS)
- What form it takes (web app / CLI / API / library)

Use this to construct search queries. Do not ask the developer anything yet.

---

**Step 2 — Run the research**

Search across these four areas. Use web search for each. Be specific — generic queries
return useless results.

**2a. Existing SaaS or hosted solutions**

> Does a product already exist that solves this problem out of the box?

Search for: `[problem description] tool`, `[problem description] software`,
`[problem description] saas`, `best [problem description] tools [current year]`

Look for: ProductHunt launches, G2/Capterra listings, Hacker News "Ask HN: tools for X"
threads.

Flag if found: name, pricing model, whether self-hosting is possible, obvious gaps.

**2b. Open source alternatives**

> Is there a project that could be used directly, forked, or extended?

Search for: `[problem description] open source`, `[problem description] github`,
`awesome [domain]` lists

Look for: GitHub repos with active maintenance (commits in last 6 months), license
compatibility, rough feature overlap.

Flag if found: repo URL, stars, last commit, license, feature gap vs what the developer
wants.

**2c. Libraries and building blocks**

> Even if nothing solves the whole problem, are there libraries that solve the hard parts?

Think about what the hard parts are (e.g., parsing, rendering, syncing, scheduling) and
search for libraries that handle them specifically.

Flag if found: library name, ecosystem (npm/pip/etc.), maintenance status, whether it
covers the core problem or just an adjacent piece.

**2d. Reference implementations**

> Has someone built something similar and written about it, even if not packaged?

Search for: `how to build [problem description]`, `[problem description] tutorial`,
`[problem description] from scratch`

Look for: blog posts, GitHub projects from individuals, Hacker News projects.

Flag if found: URL, how similar it is, whether it's worth reading before starting.

---

**Step 3 — Present findings**

Once the research is done, present a short summary structured like this:
EXISTING SOLUTIONS FOUND
SaaS / hosted:

[Name] — [what it does, pricing, gap]. URL.
None found.

Open source:

[Name] — [repo, stars, license, overlap]. URL.
None found.

Libraries:

[Name] — [what it handles, ecosystem]. URL.
None found.

Reference implementations:

[URL] — [one line on relevance].
None found.

Then ask the developer to make a build decision:

> "Based on what I found, how do you want to proceed?"
>
> - **A) Build from scratch** — nothing useful exists, or existing solutions don't fit
> - **B) Use an existing SaaS** — [name] covers most of the need; integrate instead of build
> - **C) Fork or extend an open source project** — [name] is the closest; start from it
> - **D) Build on top of existing libraries** — [names] handle the hard parts; build the rest

Wait for the developer's answer. Do not proceed to 0.4 until they decide.

---

**Step 4 — Record the decision**

Once the developer chooses, note it for AGENTS.md:

- What was found during research (brief)
- What decision was made and why
- If B, C, or D: which project/library was chosen and what it covers

If the developer chose B (use a SaaS), flag it:

> "If you're using [name] instead of building this yourself, some of the later phases may not
> apply (CI/CD, database, hosting). Keep that in mind as we go."

Then continue to 0.4.

---

### 0.4 — Repository structure

> This phase determines how the project's code is organized across repositories. The decision affects CI/CD complexity, deployment independence, and how easy it is to share code between parts of the project.

---

##### When to run this phase

Always run it, even for simple projects. The default answer ("just one repo") is fine — but it should be a deliberate choice, not an assumption.

---

##### Background: the three patterns

Before asking anything, understand the three options:

**Single repo** — one repository, one codebase, one pipeline. Everything lives together. Default for most projects.

**Monorepo (workspace/umbrella)** — one repository, multiple packages with explicit boundaries. Each package has its own `package.json`, but a coordinator (pnpm workspaces, Turborepo, Nx) manages the root. Packages can share code directly without publishing. CI can run per-package or across all. Best when you have multiple deployable units that share significant code (e.g., a Next.js app + a shared component library + a backend API).

**Multirepo** — multiple independent repositories. Each has its own CI, its own versioning, its own deploy pipeline. Packages that depend on each other do so through published versions (npm, private registry). Highest operational overhead. Best when teams or projects are truly independent and need full autonomy.

---

##### Questions

**Q1 — How many deployable units does this project have?**

> "How many independent things will this project deploy?
> 
> - A) One — a single app, API, or service
> - B) Two or three — e.g., a frontend + a backend, or an app + a shared library
> - C) More than three — multiple services, packages, or apps"

If A → single repo. No further questions needed in this phase.  
If B or C → continue to Q2.

---

**Q2 — Do these units share code?**

> "Do the different parts of the project share significant code — components, types, utilities, business logic?
> 
> - A) Yes — they share types, utilities, or UI components
> - B) No — they're mostly independent, just happen to be the same project
> - C) Not sure yet"

If A → monorepo with workspace tooling is the right default.  
If B → multirepo is viable, but single repo is still simpler unless teams differ.  
If C → default to single repo; can restructure later if needed.

---

**Q3 — Are the parts deployed or maintained independently?**

Only ask if Q1 returned B or C.

> "Can any of these parts be deployed or updated completely independently, on different schedules, by different people?
> 
> - A) Yes — they're independent enough that separate repos would feel natural
> - B) No — they move together; a change in one usually means a change in another
> - C) Mixed"

If A → multirepo may be justified. Flag the tradeoff (see below).  
If B → monorepo. Deploying together is easier when code lives together.  
If C → monorepo with workspace tooling. Let the coordinator handle selective builds.

---

##### Decision logic

|Answers|Recommended structure|
|---|---|
|Q1 = A|Single repo|
|Q1 = B/C, Q2 = A, Q3 = B/C|Monorepo with workspace tooling|
|Q1 = B/C, Q2 = B, Q3 = A|Multirepo|
|Q1 = B/C, Q2 = C or Q3 = C|Single repo now; restructure when the need is clear|

---

##### If monorepo with workspace tooling

Suggest this setup for the developer's stack:

- **Package manager:** pnpm workspaces (preferred — faster, stricter, better monorepo support than npm)
- **Build coordinator:** Turborepo (zero-config caching, per-package pipelines, works well with pnpm)
- **Structure:**

```
my-project/
├── apps/
│   ├── web/          # Next.js frontend
│   └── api/          # Backend / worker
├── packages/
│   ├── ui/           # Shared components
│   └── types/        # Shared TypeScript types
├── turbo.json
└── package.json      # Root workspace config
```

CI runs at the root but Turborepo determines which packages need to rebuild based on what changed.

---

##### If multirepo

Flag the tradeoffs before confirming:

> "Multirepo means each part lives in its own repository with its own CI pipeline and versioning. Shared code gets published as packages (to npm or a private registry) and consumed as versioned dependencies. This gives you full independence — but changes that span multiple repos require coordinated PRs, version bumps, and releases. Are you sure this fits the project, or is it complexity you don't need yet?"

If the developer confirms: note in AGENTS.md that inter-repo dependencies should be managed through a private registry or GitHub Packages, not by `npm link` or path hacks.

---

##### At the end of this phase

Record in AGENTS.md:

- Structure chosen (single / monorepo / multirepo)
- If monorepo: tooling chosen (pnpm + Turborepo recommended)
- If multirepo: how inter-repo dependencies will be managed
- If deferred: why, and what signal would trigger a revisit

Then continue to 1.1

--- 

## 1.1 — Containerization

> Do not ask the developer whether to use Docker. Work through the decision tree below
> using the project profile from phase 0.2. Present the recommendation with brief
> reasoning, then confirm before moving on.

---

#### Decision tree

**Q1 — What is the output of this project?**

- A library or package (distributed via npm, pip, or similar) → **No Docker.** Nothing
  to containerize. Skip the rest of this section.
- A native desktop or mobile app → **No Docker.** The IDE manages the environment. Skip
  the rest of this section.
- A local script with no deployment intent (throwaway tool) → **No Docker.** No
  reproducibility concern. Skip the rest of this section.
- Anything else → continue to Q2.

---

**Q2 — Where does this project deploy?**

- A platform that fully manages the runtime for all environments (Vercel, Netlify,
  Cloudflare Pages, serverless) → **No Docker.** The platform handles the runtime end
  to end. Skip the rest of this section.
- Self-managed infrastructure for all environments (VPS, Raspberry Pi, Coolify) →
  **Docker for all environments.** Continue to Q3.
- Mixed: self-managed for staging, managed platform for production → **Docker for
  staging only.** The Dockerfile lives in the repo; the production platform ignores it.
  Continue to Q3.

---

#### What Docker covers here

- **Local dev and staging:** `docker-compose.yml` at the project root. Defines the app
  and any services it depends on (database, cache, queue). One command starts the full
  environment. No local dependency installation required beyond Docker itself.

- **Production image (self-managed environments):** a `Dockerfile` with a multi-stage
  build. The dev stage is used by compose locally; the production stage produces the
  deployable image. The agent builds this based on the framework and runtime decided in
  phase 2.2.

---

#### Multiple environments with different deploy targets

A common pattern: staging runs on self-managed infrastructure (a Raspberry Pi, a VPS),
production runs on a managed platform (Vercel, Netlify, fly.io). These targets coexist
in the same repository without conflict.

Typical branch strategy:
staging  →  self-managed host (Docker)
main     →  managed platform (no Docker)

The managed platform only reads what it needs to build the project (framework config,
build command, output directory). It ignores `Dockerfile`, `docker-compose.yml`, and
`.dockerignore` entirely — these files do not interfere with the platform build in any
way. There is no need to exclude them or maintain separate branches for this reason.

CI handles routing: one job deploys to the self-managed host on pushes to `staging`,
another triggers or defers to the platform on pushes to `main`. This is covered in 1.2.

---

#### When not to use Docker

- The project is a library, package, native app, or local throwaway script.
- All environments deploy to a managed platform that handles the runtime end to end.

In all other cases, Docker is the default. This includes frontend-only projects on
self-managed infrastructure — a container gives you port assignment, process isolation,
clean restarts, and log access without polluting the host system.

---

#### File conventions

- Use `docker-compose.yml` — recognized by CI runners, Coolify, Portainer, and most
  tooling without additional configuration. The newer `compose.yml` name is not yet
  universally supported.
- Always include `.dockerignore` alongside any `Dockerfile`. The agent creates it when
  creating the Dockerfile.

---

> **Notes for the agent:** Do not create `Dockerfile`, `docker-compose.yml`, or
> `.dockerignore` during this phase. Record the decision in `AGENTS.md` and create the
> files when scaffolding the project at the start of implementation.
>
> Managed platforms (Vercel, Netlify, Cloudflare Pages, fly.io, and similar) ignore
> Docker-related files completely. Do not remove or restructure them on the assumption
> that they will cause a build conflict — they will not.

> **Dockerfile vs. docker-compose.yml:** A `Dockerfile` is only needed if the project
> builds its own image — i.e. the repository contains application code that gets
> packaged into a container. If the project only orchestrates third-party services
> (databases, caches, queues) using official images, `docker-compose.yml` is sufficient
> and no `Dockerfile` is needed. The agent determines which applies once the stack is
> decided in phase 2.2.

Then continue to 1.2

---

## 1.2 — CI/CD

> This section covers pipeline structure, runner choice, triggers, and secrets management. Branch strategy is inherited from 1.1: `staging` deploys to self-managed infrastructure, `main` deploys to the managed platform (or is the only branch if there is no managed platform).

---

#### When to skip this phase

If the project profile from 0.2 is `type = personal tool + lifespan = throwaway`, CI/CD is optional. A local `git push` and manual deploy is enough. You can return to this section if the project outlives its original scope.

For everything else, set it up from the start. Retrofitting CI is tedious.

---

#### Runner choice

For projects with a self-managed staging environment (Raspberry Pi, VPS, Coolify), use a **self-hosted runner** installed directly on that machine.

- No open ports or public IP required — the runner initiates the connection to GitHub, not the other way around.
- GitHub treats it as a normal runner. The only difference in the workflow YAML is the `runs-on` label.
- Install one runner per project with a dedicated folder and a descriptive name. Register each as a separate systemd service so they start automatically on reboot.
- If the runner machine is down, jobs queue until it comes back. This is expected behavior for self-managed infrastructure — not a reason to avoid it.

**Alternative — SSH deploy from a GitHub-hosted runner:** a job runs on GitHub's infrastructure and SSHes into the target machine to execute deploy commands remotely. This requires the target machine to be reachable from the internet (public IP or a tunnel). For most personal setups this is unnecessary complexity. Mentioned here for completeness; not the recommended default.

---

#### Pipeline structure

One workflow file per environment. Each file declares which branch it listens to — GitHub executes them independently and they never interfere with each other. A push to `staging` triggers only the staging workflow; a merge to `main` triggers only the production workflow.

Each workflow contains two sequential jobs: **test** (lint, type-check, unit tests) and **deploy** (build and release to the target environment). Deploy only runs if test passes.

For production on a managed platform (Vercel, Netlify, fly.io): the platform deploys automatically on push to `main` via its own GitHub integration. The production workflow only needs a test job — no deploy job required.

---

#### Secrets management

Not all environment variables are secrets. Distinguish between:

- **Secrets** — credentials, tokens, signing keys, anything that grants access or could cause damage if leaked. Examples: `DATABASE_URL`, `STRIPE_SECRET_KEY`, `SMTP_PASSWORD`, `JWT_SECRET`.
- **Non-sensitive config** — values that vary by environment but carry no risk if seen. Examples: `TELEGRAM_BOT_USERNAME`, `PUBLIC_API_URL`, `APP_NAME`, `PORT`, `NODE_ENV`.

Store secrets in **GitHub repository secrets** (`Settings → Actions → Secrets and variables`). They are injected as environment variables at runtime and never exposed in logs. Non-sensitive config can live in the workflow file directly or in **GitHub Actions variables** (same location, unencrypted).

**Consider avoiding writing a `.env` file to disk on the server if possible.** If an attacker gains filesystem access, a `.env` is an easy target. Instead, inject variables directly into the container from the CI environment — the runner receives them from GitHub Secrets and passes them to Docker at deploy time. For build-time secrets, Docker BuildKit supports `--mount=type=secret` to make a secret available during a build step without baking it into any image layer.

Keep a `.env.example` in the repo with every variable name and a short description, but no values. This is the canonical reference for what variables the project needs — covered in phase 3.1.

If secret count grows beyond ~10, or if the same secrets are shared across multiple repositories, consider a dedicated secrets manager (Doppler, Infisical). For most personal projects, GitHub Secrets is sufficient.

---

> **Note for the agent:** Do not create workflow files during this phase. Record the pipeline structure and runner decision in `AGENTS.md`. Create the files when scaffolding the project at the start of implementation, once the stack from phase 2.2 is known — the test and build steps depend on it.

Then continue to 1.3

---

## 1.3 — Hosting

> This phase determines where the project runs across environments. The goal is to
> establish a clear path from local development to production before any infrastructure
> is configured.
>
> Do not pick tools. Pick the shape of the deployment. Tools get decided once the stack
> is known (phase 2.2).

---

#### The three environments

Most projects need three environments. Some need fewer. None need more at this stage.

| Environment | Purpose | Who uses it |
|---|---|---|
| **Local** | Active development. Fast iteration, no stability requirement. | Developer only |
| **Staging** | Integration testing. Stable enough to test against real data and services. | Developer, sometimes collaborators |
| **Production** | Live, user-facing. Stability and uptime matter. | End users |

Personal tools and throwaway projects often collapse local and staging, or skip staging
entirely. If the project profile from 0.2 is `personal tool + throwaway`, one environment
(local) is enough. Flag it and skip to the production question.

---

#### Local development

Local development is always Docker Compose if the project has a backend or services to
run (as decided in 1.1). No questions needed here — the decision was made in 1.1.

If the project is frontend-only on a managed platform, local development is the
framework's dev server (`next dev`, `vite`, etc.). Nothing to configure at this stage.

---

#### Staging

Ask the developer:

> "Where will staging run?
>
> - A) **Self-managed machine** — a Raspberry Pi, a home server, or a spare machine
>   you control
> - B) **Cheap VPS** — a small cloud VM (Hetzner, DigitalOcean, Vultr — €3–6/month)
> - C) **Same platform as production** — a separate environment on Vercel, fly.io, etc.
> - D) **No staging** — I'll test locally and go straight to production"

Notes per option:

**A — Self-managed machine:** Full control, zero cost, zero uptime guarantee. Ideal for
personal projects where staging going down is not a problem. Requires: Docker installed
on the machine, SSH access, a way to receive webhooks from CI (either a public IP,
a tunnel like Cloudflare Tunnel, or ngrok). If the machine is a Raspberry Pi, use a
64-bit OS image and arm64-compatible base images in Docker.

**B — Cheap VPS:** Same workflow as A, but more reliable and publicly reachable without
tunneling. Recommended for internal tools or public projects where staging availability
matters. Hetzner CAX11 (arm64, 2GB RAM, €3.29/month) is a good default — small, cheap,
and Docker-ready.

**C — Platform staging environment:** Simplest to maintain. Costs extra if the platform
charges per environment. Appropriate when production is already on that platform and
environment parity matters.

**D — No staging:** Acceptable for personal tools, experiments, or projects where the
developer has high confidence from local testing. Flag the risk: production becomes the
only environment, so failures are user-facing.

---

#### Production

Ask the developer:

> "Where will production run? Pick the closest:
>
> - A) **Managed platform** (Vercel, Netlify, Cloudflare Pages/Workers, fly.io, Railway,
>   Render) — they handle servers, scaling, and deployment
> - B) **Self-managed with a panel** (Coolify, Dokku, CapRover on a VPS) — you own the
>   server, the panel handles deploy logic
> - C) **Bare self-managed** (raw VPS, Docker Compose, manual or CI-triggered deploys)
>   — full control, full responsibility
> - D) **Same machine as staging** — production and staging share a host (acceptable for
>   personal tools, not for anything user-facing)"

Notes per option:

**A — Managed platform:** Lowest operational overhead. No servers to maintain. Most
support zero-downtime deploys, preview environments, and automatic HTTPS out of the box.
The right default for public-facing projects unless there's a specific reason to
self-host. Main constraint: vendor lock-in and pricing at scale.

Common choices by project type:
- Frontend-only or Next.js: **Vercel** (zero config, tight Next.js integration)
- Full-stack with containers: **fly.io** (Docker-native, global edge, generous free tier)
- Full-stack with more control: **Railway** or **Render** (simpler than fly.io,
  slightly less control)
- Static sites: **Cloudflare Pages** (fastest CDN, free tier is generous)

**B — Self-managed with a panel:** Middle ground. You pay for a VPS (typically €5–20/month
depending on size), Coolify or Dokku handles deploy pipelines, HTTPS, and reverse proxy.
You get Docker-based deploys without writing Kubernetes. Good for developers who want
control but not raw infrastructure work. Coolify is the current recommended default —
actively maintained, supports Docker Compose projects natively, has a usable UI.

**C — Bare self-managed:** Maximum control, maximum overhead. Suitable only if there's
a specific reason (compliance, unusual hardware, cost at high scale). Requires managing
SSL renewal, reverse proxy config (Nginx, Caddy), process supervision, and log
aggregation manually or via custom automation.

**D — Same machine as staging:** Only acceptable for personal tools with no real users.
A staging deploy that goes wrong can take down production. Note it explicitly in
AGENTS.md if chosen.

---

#### Domain and DNS

Do not resolve this in full here — it's covered in 4.1. But note:

- Production should have its own domain. If the developer doesn't have one yet, flag it.
- Staging can run on a subdomain of a domain the developer already controls, or on the
  host's IP directly if staging is internal only.

---

#### Record for AGENTS.md

At the end of this phase, note:

- Local: framework dev server / Docker Compose (from 1.1)
- Staging: option chosen (A–D), machine type if A/B, platform if C
- Production: option chosen (A–D), specific platform or panel if applicable
- Any constraints: arm64 requirement, cost ceiling, existing infrastructure

Then continue to 2.1

---

## 2.1 — UX/UI design brief and DESIGN.md

> This phase produces a `DESIGN.md` file at the repository root: a machine-readable design
> system specification that any agent can read in future sessions to produce consistent UI.
> It combines exact design tokens (colors, typography, spacing, components) with prose
> rationale explaining how and why to apply them.
>
> The format follows the [google-labs-code/design.md](https://github.com/google-labs-code/design.md)
> specification. Run `npx @google/design.md lint DESIGN.md` at any time to validate token
> references and WCAG contrast ratios.

---

#### When to skip this phase

If the project has no UI — a CLI tool, a library, an API with no frontend — skip this
phase entirely. Note in AGENTS.md: `design: none — no UI in this project`.

If a `DESIGN.md` already exists (generated by Stitch, context.dev, or a previous session),
read it, note what is already defined, and ask only what is missing. Do not overwrite
decisions that are already made.

---

#### Step 1 — Triage: what does the developer already have?

Ask these three questions in order. Each answer may eliminate several later questions.

---

**Q1 — Brand assets**

> "Do you have a logo or any existing brand assets (colors, fonts, visual guidelines)?
>
> - A) Yes — I have a logo and/or defined brand colors
> - B) Partial — I have one or two colors in mind but nothing formal
> - C) No — starting from scratch"

If A: ask the developer to share the assets or describe the colors. Extract the primary
palette from them. Skip the mood and color questions below — the brand already answers them.

If B: note the colors and continue to Q2. The mood questions will fill the gaps.

If C: continue to Q2.

---

**Q2 — Reference sites (positive)**

> "Name one or two sites or apps whose design you like — they don't need to be in the
> same space as your project. Just something that feels right visually."

This is the most useful input you will get. From a good reference you can extract
palette, typographic feel, density, and tone without asking for each separately.

If the developer names a site with a public URL: suggest going to
[context.dev/free-tools/design-md-generator](https://www.context.dev/free-tools/design-md-generator)
to generate a DESIGN.md directly from that URL. If they do, skip steps 2 and 4 entirely
— the generated file replaces both the Q&A and the design prompt. Continue at step 5.

If the reference is an app, a product, or anything without a scrapable public URL: fetch
or search for screenshots. Identify the dominant palette (1–2 primary colors, background
tone, accent), whether it skews light or dark, and whether it feels dense or spacious.
Carry these observations into the design brief and continue to Q3.

If they cannot name any: continue to Q3 and the structured questions below.

---

**Q3 — Reference sites (negative)**

> "Is there a visual style or specific site you want to avoid? Something that would feel
> wrong for this project?"

Note what to avoid. Even a vague answer ("I don't want it to look like a corporate
enterprise dashboard") is useful — it rules out an entire aesthetic direction.

---

#### Step 2 — Structured questions (fill remaining gaps only)

Ask only what the answers to Q1–Q3 have not already resolved. One question at a time.

**Mood and personality**

> "If this UI were a person walking into a room, which fits best?
>
> - A) Reliable and professional — confident, no surprises, inspires trust
> - B) Friendly and approachable — warm, human, a bit conversational
> - C) Minimal and focused — out of the way, lets the content breathe
> - D) Elegant and premium — refined, considered, high-end feel
> - E) Bold and direct — strong opinions, high contrast, energetic"

---

**Light / dark / both**

> "Dark mode, light mode, or both?
>
> - A) Light only
> - B) Dark only
> - C) Both — system preference or user toggle"

---

**Density**

> "How much information fits on screen at once?
>
> - A) Dense — lots of data, tight spacing, efficient use of space (dashboards, admin panels)
> - B) Spacious — generous padding, clear hierarchy, room to breathe (marketing, editorial)
> - C) Balanced — standard SaaS: not cramped, not lavish"

---

**Shape**

> "Does this feel more like a web app or a website?
>
> - A) App — functional UI, persistent navigation, feels like software
> - B) Site — pages, sections, scrolling, closer to editorial or marketing
> - C) Hybrid — landing page or marketing site + an authenticated app section"

---

**Viewport priority**

> "What is the primary viewport?
>
> - A) Desktop first — most users will be on a large screen
> - B) Mobile first — most users will be on a phone
> - C) Equal — needs to work well on both from the start"

---

**Component system**

> "How do you prefer to work with UI components?
>
> - A) Full control — I want to own the code, customize everything, no black boxes
> - B) Productive defaults — I want pre-built components that look good out of the box
>   and are easy to theme
> - C) No preference — choose what fits the stack"

The agent will select the specific component library in phase 2.2 once the framework
is decided. Record the preference here as a constraint for that decision.

---

#### Step 3 — Synthesize the design brief

Once you have enough answers, write a design brief in your context (not yet to a file).
Keep it tight: 3–4 sentences on personality and tone, then a bullet list of concrete
decisions.

Example:

> This UI prioritizes clarity over expressiveness. It should feel like a well-made tool:
> confident, fast, and out of the way. The aesthetic is minimal and light with a single
> strong accent color for interactive elements.
>
> - Mode: light, with dark mode via system preference
> - Density: balanced — standard SaaS spacing
> - Shape: desktop-first app
> - Component system: shadcn/ui + Tailwind
> - References: Linear (structure and speed), Vercel dashboard (monochrome base + accent)
> - Avoid: bright multicolor palettes, heavy gradients, card-heavy layouts

---

#### Step 4 — Generate the design prompt

From the brief, write a ready-to-use prompt for the developer to paste into their design
tool of choice. The prompt should be self-contained — it should work without any
additional context.

Structure the prompt as:

1. What the product does (one sentence, from phase 0.1)
2. Personality and tone
3. Visual references (positive and negative)
4. Concrete constraints: mode, density, viewport, component system
5. What to generate: a homepage, dashboard, or onboarding screen — whichever makes most
   sense as a first screen to design

Tell the developer:

> "Here is a prompt you can paste into Stitch (stitch.withgoogle.com), v0 (v0.dev), base44,
> or Claude.ai. Paste it, see what comes back, and let me know if it feels right or what
> you would change."

Wait for the developer to return with feedback. If they want to adjust something, update
the brief and regenerate the prompt. If they are happy with what the tool produced,
continue to step 5.

---

#### Step 5 — Validate and finalize DESIGN.md

At this point the developer may already have a `DESIGN.md` generated by an external
tool. Check which case applies before doing anything:

**The developer used Stitch and exported DESIGN.md from the mockup:** use that file
as-is. Do not regenerate. Check it with the linter and fix any errors.

**The developer used context.dev and pasted the result back in Q2:** adapt it to the
actual project — update the name, description, and any colors or tokens that belong to
the reference site and not to this project. The structure and prose rationale are worth
keeping — they are more complete than anything the Q&A process would produce on its own.

**Neither of the above:** generate the file from scratch at the repository root,
following the [google-labs-code/design.md](https://github.com/google-labs-code/design.md)
spec. Consult the full spec with `npx @google/design.md spec` before writing. The file
must include YAML front matter with exact token values and a markdown body with sections
in this order: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes,
Components, Do's and Don'ts.

Once the file exists and is adapted to the project, validate it regardless of its origin:

    npx @google/design.md lint DESIGN.md

Fix errors (broken token references, contrast failures below WCAG AA). Warnings are
informational — address them if they point to a real problem, skip them if the decision
is intentional.

---

#### Step 6 — Record in AGENTS.md

Add to the AGENTS.md section for this project:

```
## Design
DESIGN.md: [repo root]/DESIGN.md
Component system: [shadcn/ui + Tailwind | DaisyUI | Mantine | other]
Mode: [light | dark | both]
Viewport priority: [desktop | mobile | equal]
Primary references: [list]
```

Any agent working on UI in future sessions must read `DESIGN.md` before generating any
component, layout, or style.

---

Then continue to 2.2

---

## 2.2 — Technologies and frameworks

> This phase determines the technical stack by layer. Its output is a set of concrete, locked-in choices — not a shortlist. By the end, the agent must be able to scaffold the project without asking any further technology questions.
>
> Do not re-ask anything already established in phases 0 or 2.1. Check the project profile from 0.2 and the DESIGN.md from 2.1 before asking anything.

---

#### Before asking anything

From prior phases you already know:

- **Project shape** (0.1/0.2): web app / CLI / API / library / desktop / mobile
- **Backend presence** (0.1): frontend-only / full-stack / API-only
- **Component system** (2.1): shadcn/ui+Tailwind / DaisyUI / Mantine / none
- **Developer's language preference**, if stated anywhere

Use this to skip entire layers. A CLI tool skips all frontend questions. A frontend-only web app skips backend framework questions. A library skips both.

---

#### Step 1 — Language and ecosystem anchor

This is the single most consequential decision. Everything else follows from it.

> "What language do you want to build this in? Pick the closest:
>
> - A) TypeScript / JavaScript (Node.js, Bun, or Deno)
> - B) Python
> - C) Go
> - D) Rust
> - E) Ruby
> - F) Swift (Apple platforms only)
> - G) Other / I don't know yet"

If the developer named a specific framework in phase 0 (e.g. "a Next.js app"), the language is already decided. Do not ask this question — carry the answer forward.

If G: ask what the project does and recommend the most natural fit. A systems tool with performance requirements → Go or Rust. A data-heavy backend or ML-adjacent project → Python. A web app with no strong preference → TypeScript. Then treat the recommendation as the answer and continue.

---

#### Step 2 — Frontend framework

Skip this step entirely if:
- Project has no UI (API-only, CLI, library)
- Language chosen in Step 1 is not TypeScript/JavaScript (unless the developer explicitly wants a JS frontend with a non-JS backend)

---

**If TypeScript / JavaScript:**

> "What kind of frontend are you building?
>
> - A) A full web app where pages and routing matter (blog, SaaS, marketing site with auth)
> - B) A single-page app or dashboard (heavy client-side state, no meaningful URLs)
> - C) A mostly static site with some dynamic content
> - D) Something embedded or minimal — no framework needed"

Route based on the answer:

**A → Meta-framework.** Present these options:

| Option | Best when |
|---|---|
| **Next.js** | Default for React-based full-stack apps. Large ecosystem, tight Vercel integration, RSC support. |
| **SvelteKit** | Smaller bundle, less boilerplate, excellent DX. Good if the developer is comfortable with Svelte or wants to learn it. |
| **Nuxt** | Same as Next.js but for Vue. Only if the developer has a strong Vue preference. |
| **Remix** | Strong opinion on web fundamentals (forms, loaders, nested routes). Good for data-heavy apps that benefit from progressive enhancement. |
| **Astro** | Content-heavy sites (docs, blogs, portfolios) where most pages are static and interactivity is optional. |

If the developer has no strong preference and the project profile is a standard SaaS or web app: recommend **Next.js**. It is the safest default — widest ecosystem, most compatible with the integrations in phase 2.4, fewest surprises.

**B → SPA.** The meta-framework handles routing server-side, which is overkill for a fully client-rendered dashboard. Ask:

> "Are you using an existing backend or API, or will you need to build one?
>
> - A) Existing API (my own or a third-party one)
> - B) I need to build a backend too"

If A: use Vite + React (or Vite + Vue / Svelte if the developer has a preference). No SSR, no meta-framework complexity.

If B: reconsider a meta-framework (option A above). A full-stack SPA with a separate backend is usually more complex than it needs to be.

**C → Static with islands.** Default to **Astro**. It generates static HTML by default and lets you add interactive components in React, Vue, or Svelte where needed. Zero JS overhead unless you opt in.

**D → No framework.** Note it and continue to Step 3.

---

**If Python, Go, Rust, Ruby, or other backend language with a web UI:**

These stacks typically render HTML server-side or serve a separate JS frontend. Ask:

> "Where does the UI live?
>
> - A) Server-rendered HTML (templates, htmx, or similar)
> - B) Separate frontend (a JS app that calls this backend's API)"

If A: the UI framework decision happens at the backend level (see Step 3). No separate frontend framework needed.

If B: go back and run the TypeScript frontend questions above for the frontend layer, then continue with the non-JS backend in Step 3.

---

#### Step 3 — Backend framework

Skip this step if the project is frontend-only with no custom backend.

Before routing by language, determine whether the backend will be built from scratch or already exists as a platform:

> "Will you build the backend, or will you use an existing platform that exposes an API (an e-commerce engine, a headless CMS, an ERP)?
>
> - A) I'll build it — custom code, my own database
> - B) An existing platform handles it — I'll call its API from the frontend"

If A: continue to the language-based options below.

If B: go to **Headless platform** section below, then skip the rest of Step 3.

---

#### Headless platform

The backend already exists. Your work is the frontend and, optionally, a thin BFF (Backend For Frontend) to protect API keys or handle webhooks.

Identify the platform category:

| Category | Options |
|---|---|
| **E-commerce** | Medusa (self-hosted), Shopify (SaaS), Saleor (self-hosted), Vendure (self-hosted), BigCommerce (SaaS) |
| **Headless CMS** | Sanity, Contentful, Strapi (self-hosted), Payload (self-hosted) |
| **ERP / vertical platform** | Odoo, ERPNext, or any system with a documented API |

Ask which platform applies. If the developer already named it earlier, carry the answer forward without asking.

Then ask one follow-up:

> "Does your frontend need to call the platform API directly from the browser, or should API calls go through a server layer you control?
>
> - A) Directly from the browser — the platform allows it and no sensitive keys are involved
> - B) Through a server layer — I need to protect API keys, transform responses, or handle webhooks"

If A: no custom backend needed. Note it and continue to Step 4.

If B: a thin BFF is needed. This is not a full backend — it is a small server layer with one or two responsibilities. Recommended approach by language:

- TypeScript → **Hono** (edge-friendly, minimal) or Next.js API routes if already using Next.js
- Python → **FastAPI** with a minimal route set
- Other → the standard HTTP library for the chosen language is sufficient

Note in `AGENTS.md` that the BFF has a narrow scope. It must not grow into a general-purpose backend.

**Note on phase 2.3 (database):** most headless platforms manage their own database. Ask:

> "Does the platform manage its own data entirely, or will you store additional data outside it (user preferences, custom content, orders not handled by the platform)?"

- Platform manages everything → skip phase 2.3.
- Additional data needed → continue to 2.3 as normal.

**Note on self-hosted platforms (Medusa, Saleor, Strapi, Payload, Odoo):** these require a server and a database. Their hosting decisions overlap with phases 1.1 and 1.3. Flag it:

> "You're self-hosting [platform]. Its infrastructure decisions (server, database, Docker) are covered in phases 1.1 and 1.3. We'll revisit them there — for now, just note the platform name."

Record in `AGENTS.md`:

```
Backend: headless platform — [platform name]
BFF: [Hono | Next.js routes | FastAPI | none]
Platform hosting: [self-hosted | SaaS]
Additional database: [yes — continue to 2.3 | no — skip 2.3]
```

---

Based on the language from Step 1:

---

**TypeScript / JavaScript:**

| Option | Best when |
|---|---|
| **Next.js API routes / Route Handlers** | Already using Next.js for the frontend. Avoid adding a separate backend process. |
| **tRPC** | Full-stack TypeScript monorepo. End-to-end type safety without a REST or GraphQL layer. |
| **Hono** | Lightweight, edge-ready API server. Good for Cloudflare Workers, Bun, or when you want a minimal HTTP layer. |
| **Fastify** | High-performance Node.js API. Good for standalone backend services that need speed and a plugin ecosystem. |
| **Express** | Only if the developer has strong existing familiarity. No other reason to choose it in a new project. |

If the project is a Next.js full-stack app: default to **Next.js API routes / Route Handlers**. No separate backend process. Ask only if the developer explicitly wants a separate service.

If the project needs a standalone backend: ask whether edge deployment matters.

> "Will this backend run on edge runtimes (Cloudflare Workers, Vercel Edge), or on a standard Node.js / container environment?
>
> - A) Edge — must run on Cloudflare Workers or similar
> - B) Standard Node.js or container"

If A → **Hono** (edge-native, small, no Node.js dependencies).
If B → **Fastify** (performance) or tRPC (if type safety across monorepo matters more than performance).

**BaaS as a backend alternative:** if the project has no complex server-side logic — mostly CRUD, auth, and file storage — consider skipping a custom backend entirely.

> "Does your backend need custom business logic, or is it mostly reads, writes, and auth?
>
> - A) Custom logic — workflows, calculations, integrations, scheduled jobs
> - B) Mostly CRUD and auth — standard data operations"

If B: present these alternatives:

| Option | Best when |
|---|---|
| **Supabase** | PostgreSQL-based. Open source, self-hostable. Includes auth, storage, realtime, edge functions. |
| **PocketBase** | Single binary, SQLite-based. Extremely easy to self-host. Good for small to medium projects. |
| **Appwrite** | Docker-based. More opinionated than PocketBase, more features. Good if you want a full self-hosted BaaS. |

If any of these is chosen, note it in AGENTS.md and skip the database phase (2.3) — the BaaS provides the database layer.

---

**Python:**

| Option | Best when |
|---|---|
| **FastAPI** | Default for API-first backends. Async, typed, fast, great OpenAPI support. |
| **Django** | Full-stack with ORM, admin panel, and batteries included. Good for content-heavy apps or when an admin interface matters. |
| **Flask** | Minimal. Only if the developer wants explicit control over every component and FastAPI is not a fit. |
| **Django + HTMX** | Server-rendered UI without a JS frontend. Good for internal tools or content sites that don't need React. |

Default recommendation: **FastAPI** for API-only backends, **Django** for full-stack apps with admin needs.

---

**Go:**

| Option | Best when |
|---|---|
| **Standard library + net/http** | CLI tools, small services, maximum simplicity. |
| **Chi** | Lightweight router, stays close to the standard library. |
| **Echo** | More features, good middleware ecosystem. |
| **Gin** | Similar to Echo. Large community. |

For most Go web projects: **Chi** or the standard library. Go's standard library is capable enough that a full framework is rarely worth the dependency.

---

**Rust:**

| Option | Best when |
|---|---|
| **Axum** | Default. Tokio-native, async, composable, well-maintained. |
| **Actix-web** | Higher raw performance, more mature ecosystem. Slightly more complex. |

Default: **Axum** unless benchmark results specifically justify Actix-web.

---

**Ruby:**

| Option | Best when |
|---|---|
| **Rails** | Default. Full-stack, conventions-first, excellent for CRUD-heavy apps. |
| **Sinatra** | Minimal API or microservice. Only if Rails is genuinely overkill. |

---

#### Step 4 — Runtime (TypeScript/JavaScript only)

Skip if language is not TypeScript/JavaScript.

The runtime choice affects package compatibility, performance, and deployment targets.

> "Which runtime will this project use?
>
> - A) Node.js — the universal default, widest compatibility
> - B) Bun — drop-in Node.js replacement, significantly faster installs and startup
> - C) Deno — security-first, built-in TypeScript, native Web APIs"

Notes per option:

**A — Node.js:** Default. Every library works. Most deployment targets assume it. No surprises.

**B — Bun:** Compatible with most Node.js packages. Noticeably faster for local dev and CI. Some edge cases with native modules. If the developer values speed and is comfortable debugging occasional compatibility issues: a good choice. Not yet the default for production-critical projects.

**C — Deno:** Different module system (URL imports, JSR). Native TypeScript without a build step. Best for projects that want Web API parity (Fetch, WebSockets) without polyfills. Most suitable for edge functions or CLI tools, not for projects that depend heavily on npm packages.

If the developer has no preference and the project is a standard web app: default to **Node.js**.

---

#### Step 5 — Mobile framework (if applicable)

Skip unless project shape from 0.2 is mobile.

> "Are you building for one platform or both?
>
> - A) iOS only
> - B) Android only
> - C) Both iOS and Android"

If A (iOS only): **Swift + SwiftUI**. Native stack, best platform integration, Apple's own tools.

If B (Android only): **Kotlin + Jetpack Compose**. Native stack, Google's recommended approach.

If C (both): ask whether native or cross-platform:

> "Do you need native performance and full platform APIs, or is cross-platform development speed more important?
>
> - A) Native performance and deep platform integration
> - B) Shared codebase, faster development"

If A → build two separate native apps (Swift for iOS, Kotlin for Android). Expensive but highest quality.

If B → cross-platform framework. Options:

| Option | Best when |
|---|---|
| **React Native + Expo** | TypeScript already in the stack. Large ecosystem. Expo simplifies distribution and OTA updates. |
| **Flutter** | No JS/TS requirement. Excellent rendering consistency across platforms. Strong for UI-heavy apps. |
| **Kotlin Multiplatform** | Shared business logic, native UIs per platform. Good if the team is Kotlin-first. |

Default for cross-platform: **React Native + Expo** if the project already uses TypeScript, **Flutter** otherwise.

---

#### Step 6 — Desktop framework (if applicable)

Skip unless project shape from 0.2 is desktop.

> "Do you want to use web technologies (HTML/CSS/JS) or native code for the desktop UI?
>
> - A) Web technologies — I want to reuse frontend skills or share code with a web app
> - B) Native — best platform integration, smallest binary, no web overhead"

If A:

| Option | Best when |
|---|---|
| **Tauri** | Rust backend + web frontend. Small binary, low memory, secure. Strong default for new projects. |
| **Electron** | Widest ecosystem, proven at scale (VS Code, Slack, Figma). Heavy binary. Only if Tauri's constraints are a problem. |

Default: **Tauri**. Significantly smaller and more performant than Electron. The tradeoff is a Rust backend — if the developer is not comfortable with Rust, flag it.

If B:
- macOS → Swift + SwiftUI
- Windows → C# + WinUI 3 or WPF
- Cross-platform → Qt (C++) or Slint (Rust)

---

#### At the end of this phase

Record in `AGENTS.md` under `## Stack`:

Then continue to 2.3

---

## 2.3 — Database

> This phase determines what database engine the project uses and how the application
> accesses it. It has two parts: engine selection and access layer. Both decisions depend
> heavily on what was decided in phases 0.2 (project profile), 1.1 (containerization),
> 1.3 (hosting), and 2.2 (stack).

---

#### When to skip this phase

If phase 2.2 concluded with a BaaS (Supabase, PocketBase, Appwrite) or a headless
platform that manages its own data entirely, the database is already decided. Note it in
`AGENTS.md` and skip to 2.4.

If the project has no persistent data at all (a CLI tool, a stateless proxy, a pure
compute script), skip this phase and note it.

---

### Part A — Engine

#### Fast track

For most projects, the right database is **PostgreSQL**. Before asking anything else,
check whether any of the following conditions apply. If none do, go straight to
PostgreSQL and skip to Part B.

Fast-track disqualifiers — ask yourself:

- Is this a single-process app on a single server — a bot, a personal script with persistence, a small internal API, a desktop app? → Consider **SQLite** first.
- Is the only data need sessions, caching, rate limiting, or a job queue? → Consider **Redis / Valkey** instead of a relational DB.
- Does the project store AI embeddings or run semantic search? → PostgreSQL with **pgvector** extension, or a dedicated vector store.
- Is the data model genuinely non-relational — deeply nested documents with no fixed schema, varying fields per record? → Ask the developer before assuming NoSQL.

If none of these apply: **PostgreSQL is the engine. Move to Part B.**

---

#### Q1 — Only ask if a fast-track condition was triggered

> "Based on what you've described, [condition] suggests [engine] might fit better than
> PostgreSQL. Does that match what you have in mind, or do you want to stick with
> PostgreSQL?"

Do not present a menu of every database option. Present the specific alternative that the
condition triggered, explain why in one sentence, and let the developer confirm or
override.

---

#### Engine reference

Use this table to guide the recommendation if a fast-track condition applies. Do not
present this table to the developer — it is for the agent's internal routing only.

| Condition | Engine | Notes |
|---|---|---|
| Single-process app on a single server (bot, personal script, small internal API, desktop app) | **SQLite** | Embedded, zero infrastructure. The right call when one process owns the data and concurrency is not a concern. Also consider Turso (distributed SQLite, libSQL) if edge access is needed. |
| Cache, sessions, rate limiting, pub/sub | **Redis** or **Valkey** | Valkey is the open-source Redis fork post-license change. Upstash for serverless/edge. May complement PostgreSQL rather than replace it. |
| AI embeddings / semantic search | **pgvector** (PostgreSQL extension) | Avoid a separate vector store until pgvector demonstrably doesn't meet performance needs. |
| Genuine document model (flexible schema, deeply nested) | **MongoDB** | Only if the data truly doesn't fit a relational model. Atlas for managed hosting. |
| Time-series metrics at volume | **TimescaleDB** (PostgreSQL extension) or InfluxDB | TimescaleDB preferred — stays in the PostgreSQL ecosystem. |

---

#### Hosting the database

Do not re-ask what was decided in phase 1.3. Apply these rules based on the hosting
decision already recorded:

**If production is on a managed platform (Vercel, fly.io, Railway, Render):**
Use a managed database service. These platforms have native integrations:
- PostgreSQL → Neon (serverless, branching, free tier), Supabase (includes auth and
  storage), Railway Postgres, fly.io Postgres.
- Redis → Upstash (serverless, edge-compatible).

**If production is self-managed (Coolify, bare VPS, Raspberry Pi):**
Run the database as a Docker service alongside the application.

> ⚠️ If the database runs in Docker, named volumes are not optional. A container
> without a named volume loses all data every time it is recreated. The `docker-compose.yml`
> must declare a named volume for the database service's data directory. Coolify handles
> this automatically for managed services; if writing the compose file manually, do not
> skip it.

Example of what must be present (do not include in the final compose file without
adapting to the actual project):

```
---

Then continue to 2.4
---

## Annex B — Distribution

> Run this annex if or when you decide to make the project available as a distributable package. It is not necessarily part of the initial jumpstart flow.
>
> If distribution is clear from the start (e.g. "I'm building an npm library"), run this annex immediately after phase 0.2. Otherwise, come back to it when the need arises.

---

### Before asking anything

From the project profile (phase 0.2) and the current state of the project, you already know:

- What the project produces (a library, a CLI, an app, a backend)
- What runtime or ecosystem it targets (Node.js, Python, Rust, Swift, etc.)
- Whether it's already live or still in early development

Use this to skip irrelevant distribution channels. Do not present options that can't apply.

---

### Q1 — What are you distributing?

> "What form does the distributable take? Pick the closest:
>
> - A) A library or package (imported by other code)
> - B) A CLI tool (run from the terminal)
> - C) A desktop app (macOS, Windows, Linux)
> - D) A mobile app (iOS, Android)
> - E) Something else"

Route to the relevant section below based on the answer. More than one may apply — e.g. a CLI that's also published as a library.

---

### A — Library or package

**Channels:** npm (JavaScript/TypeScript), PyPI (Python), crates.io (Rust), JSR (Deno/modern JS).

**Decisions to make:**

- **Package name:** is the name available on the target registry? Check before committing.
- **Scope:** public unscoped (`my-lib`), public scoped (`@myname/my-lib`), or private? Scoped is recommended — avoids naming conflicts and signals authorship.
- **Versioning:** use semantic versioning. Automate it — do not bump versions manually. Tools: `changesets` (monorepos and libraries), `semantic-release` (CI-driven, fully automated), `np` (simple interactive releases for single packages).
- **What goes in the package:** configure `files` in `package.json` (or equivalent). Only ship what consumers need — no source maps, no test files, no config files unless they're part of the API.
- **Dual formats (JS/TS only):** does this need to ship both ESM and CJS? Most modern tooling (Vite, tsup, unbuild) handles this. Decide upfront — retrofitting dual output is painful.
- **Types:** always ship TypeScript declarations (`.d.ts`). If the source is JS, generate them. If the source is TS, emit them as part of the build.

**CI additions:**

- Publish step triggered on version tag (`v*.*.*`) or on merge to `main` with a new version.
- Requires a registry token stored as a CI secret (`NPM_TOKEN`, `PYPI_TOKEN`, etc.).
- Run tests and build before publishing. Never publish from a local machine.

---

### B — CLI tool

**Channels depend on the ecosystem:**

| Runtime | Primary channel | Secondary |
|---|---|---|
| Node.js | npm (with `bin` field) | Homebrew tap |
| Python | PyPI (with entry point) | Homebrew tap, pipx |
| Go | Homebrew tap, GitHub Releases | winget, Scoop |
| Rust | crates.io, GitHub Releases | Homebrew tap, winget |
| Compiled binary | GitHub Releases | Homebrew tap, Scoop |

**Decisions to make:**

- **Installation method for end users:** what's the one-liner you want people to run? Work backwards from that.
- **Homebrew tap vs formula:** a personal tap (`homebrew-tap` repo under your GitHub account) is the fastest path. Getting into `homebrew-core` requires broader adoption and a review process — not a day-one decision.
- **Binary distribution:** if the CLI is compiled (Go, Rust), use GitHub Releases with pre-built binaries per platform. Tools like `goreleaser` (Go) or `cargo-dist` (Rust) automate cross-platform builds and release uploads.
- **Versioning:** same as libraries. Tag-driven releases.

**CI additions:**

- Cross-platform build matrix if distributing binaries (linux/amd64, darwin/arm64, darwin/amd64, windows/amd64).
- Release job triggered on version tag.
- Homebrew tap update can be automated: `goreleaser` and `cargo-dist` both support tap formula generation.

---

### C — Desktop app

**Channels by platform:**

| Platform | Primary | Notes |
|---|---|---|
| macOS | Direct download (DMG/PKG), Mac App Store | App Store requires Apple Developer account ($99/yr), sandboxing, review |
| Windows | Direct download (EXE/MSI), Microsoft Store, winget | winget submission via GitHub PR to `microsoft/winget-pkgs` |
| Linux | AppImage, Flatpak, Snap, `.deb`/`.rpm` | AppImage is the most portable; Flatpak via Flathub has the widest reach |

**Decisions to make:**

- **Code signing:** mandatory for macOS (Gatekeeper blocks unsigned apps) and Windows (SmartScreen warnings). Requires certificates — Apple Developer Program for macOS, a code signing cert for Windows.
- **Auto-update:** if distributing outside an app store, you need an update mechanism. Most Electron/Tauri frameworks include one.
- **App stores vs direct:** stores add friction (review, fees, restrictions) but provide discoverability and trust. Direct distribution is faster but requires more trust-building.

**CI additions:**

- Platform-specific build jobs. Signing keys and certificates stored as CI secrets.
- Release artifacts uploaded to GitHub Releases.
- If targeting an app store: a separate submission step in CI or a manual upload gate.

---

### D — Mobile app

> If the project is a mobile app, this should have been flagged in phase 0.2. Distribution for mobile is not optional or late-stage — it is part of the pipeline from day one.

**Channels:**

- **iOS:** App Store (mandatory for public distribution). Requires Apple Developer account ($99/yr). TestFlight for beta distribution.
- **Android:** Google Play Store (recommended). Requires Google Play Developer account ($25 one-time). Internal testing, closed testing, and open testing tracks available before public release.

**CI additions (mandatory from the start):**

- Code signing setup: provisioning profiles and certificates (iOS), keystore (Android). Stored as CI secrets — never committed.
- Build pipeline: generates signed `.ipa` (iOS) or `.aab` / `.apk` (Android).
- Automated delivery: Fastlane is the standard tool for submitting builds to TestFlight and Play Console from CI.

---

### At the end of this annex

Record in `AGENTS.md` under a `## Distribution` section:

- Which channels apply
- Versioning and release tooling chosen
- What CI jobs were added
- Any accounts, certificates, or tokens that need to be set up manually (do not store values — list what is needed and where it should go)