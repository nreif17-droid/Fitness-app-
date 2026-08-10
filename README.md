# fitness-platform

> Working repo name — rename before you push if you land on a brand name.

An all-in-one, AI-driven fitness platform: training, nutrition, sleep, and goal
tracking in one system, with an AI coach that builds and adapts personalized
programs across every training modality using a standing knowledge base of
sports-science principles.

Built on **Base44** (AI-generated full-stack app: managed backend, auth,
storage, hosting) with this repo as the source of truth for product vision,
data model, domain logic, and the training knowledge base — connected to
Base44 via its GitHub integration, and worked on here via Claude Code.

## Folder structure

This follows the same shape as your other repos (`vision/ domains/ logs/ agents/`):

```
/vision/            — what this is, who it's for, what makes it different
/domains/           — the functional areas of the product
  /training/
    knowledge-base/ — the 12-week program library, one file per modality,
                       plus evidence updates and special populations —
                       this is what the AI coach draws on to build programs
  /nutrition/
    knowledge-base/ — energy balance, protein, body composition,
                       supplements, and the scope-of-practice limits
  /sleep/
    knowledge-base/ — sleep science and the recovery/readiness logic that
                       drives autoregulation
  /goals/
  /community/        — coursework/live calls/forum for the owner's coaching
                       practice (DEFERRED — spec only, do not build)
  /ai-coach/         — how personalization/orchestration actually works,
                       plus the adherence/behavior-change principles
  /data/             — data model / schema
  /platform/         — Base44 architecture notes + the build playbook
/agents/             — agent persona + operating instructions for Claude Code
/logs/               — decisions and build history
```

## Where to start reading

1. `vision/product-vision.md` — the pitch and what "done" looks like
2. `domains/data/data-model.md` — the core entities everything else hangs off
3. `domains/ai-coach/orchestration.md` — how the coach actually personalizes
4. `domains/platform/base44-build-playbook.md` — **how to actually get quality
   output from Base44**, and where its limits sit relative to this product
5. `agents/CLAUDE.md` — how the agent working in this repo should operate

**Before building anything that outputs nutrition advice**, read
`domains/nutrition/knowledge-base/03-safety-and-scope.md` — it describes limits the
product must enforce, and an open legal question that isn't settled.

## Getting this live

This scaffold was built in a sandboxed environment without GitHub/network
access, so the next steps are manual, on your end:

1. `git init`, review, `git add -A && git commit -m "scaffold"`.
2. Create the GitHub repo under `nreif17-droid` (or wherever you want this
   to live) and push.
3. In Base44: **Connect → GitHub**, point it at this repo. Base44's GitHub
   integration keeps the platform's generated app and your repo in sync —
   confirm current behavior in `docs.base44.com` before your first push,
   since exact sync semantics (one-way vs. two-way, which branch) are a
   platform detail worth verifying rather than assuming.
4. Open this repo in Claude Code (mobile or desktop) the same way you work
   your other repos, with `agents/CLAUDE.md` as the operating brief.

## Status

Scaffold only — vision, data model, and knowledge base are drafted;
no app has been generated in Base44 yet. See `logs/decisions.md` for the
assumptions baked into this first pass and `logs/build-log.md` to start
tracking sessions.
