# Platform: Base44

## What Base44 actually is (verified Aug 2026 — recheck docs.base44.com before
relying on specifics, since this is a fast-moving product)

Base44 is an AI-powered, no-code/low-code app builder (acquired by Wix in
2025). Core mechanics:

- **Chat-based generation** — you describe the app in natural language and
  it generates frontend, backend, database schema, and business logic.
- **Managed backend included** — database, authentication, file storage,
  hosting are handled by the platform; no separate DevOps setup.
- **"Start visually, drop into code, or do both"** — it's not exclusively
  no-code. There's a live visual editor/chat interface, and on paid plans,
  code-level access and **code export** for full ownership.
- **GitHub integration** — Base44 can connect to a GitHub repo, which is the
  intended bridge between this repo and the live app. The exact sync
  semantics (whether it's one-way push-to-deploy, two-way sync, which
  branch, how conflicts resolve) should be confirmed directly against
  current docs before depending on a workflow — don't assume.
- **Built-in integrations** — the docs mention Slack, Google Workspace,
  GitHub, and 20+ services including Stripe and email providers as
  connectors, plus the ability to build AI agents inside an app or
  cross-app "Superagents."
- **Schema-first** — Base44 reportedly builds the data model/schema before
  the UI, which lines up well with treating `domains/data/data-model.md` in
  this repo as the thing to get right early.

## What this means for how this repo is used

This repo is **not** the deployed application — Base44 is. This repo is:

1. The **spec and knowledge base** that prompts to Base44 should be built
   from, so that what gets generated matches a consistent product vision
   and data model instead of drifting prompt-to-prompt.
2. Once code-level access/export is in play, potentially the actual
   **synced codebase**, edited here and pushed through the GitHub
   connection.

Practically, that means work in this repo tends to fall into two modes:

- **Spec mode** — writing/refining the docs in `vision/`, `domains/`, and
  `domains/training/knowledge-base/` that get turned into a Base44 prompt,
  by hand or by an agent drafting the prompt from these docs.
- **Code mode** — once there's an exported/synced codebase in this repo,
  making direct code changes the way any other repo in this ecosystem is
  worked (matching the pattern already used for `personal-os` and the other
  project repos).

## Open questions to resolve before building starts in earnest

- Confirm current GitHub sync behavior and whether it's bidirectional.
- Confirm what stack Base44 actually exports (framework, language, DB) —
  this determines what "professional master-level developer" competence
  needs to cover in code mode, and shouldn't be assumed from general
  full-stack knowledge until verified.
- Confirm current pricing tier needed for code export / GitHub connection,
  since that gates whether code mode is available at all yet.
