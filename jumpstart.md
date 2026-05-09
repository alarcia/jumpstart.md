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

Then continue to Phase 1.

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