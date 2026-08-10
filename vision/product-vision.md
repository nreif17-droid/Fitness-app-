# Product Vision

## The pitch

One system that tracks a person's training, nutrition, sleep, and goals
across any fitness modality, and uses an AI coach — grounded in a real
knowledge base of sports-science principles, not generic advice — to figure
out what someone is actually trying to achieve, where they presently are,
and how to get there. It builds the program, adapts it as the person's data
comes in, and keeps doing that indefinitely rather than handing over a
static 12-week PDF and walking away.

## What makes this different from a normal fitness app

Most fitness apps are one of two things: a logging tool with no intelligence
behind it (you track, it graphs), or a fixed program with no adaptation
(you follow, it doesn't listen). This is meant to be neither — the AI coach
sits on top of a genuine training-methodology knowledge base (periodization,
volume landmarks, autoregulation, deload logic — the same principles that
govern the modality library in `domains/training/knowledge-base/`) and uses
the person's actual logged data — training, sleep, nutrition, how sessions
felt — to decide what happens next, the way a real coach would, instead of
running a fixed template regardless of what's actually happening.

## Core loop

1. **Onboarding** — the coach establishes: what does this person want, what's
   their current level/history, what constraints do they have (time,
   equipment, injuries, competing goals).
2. **Program generation** — the coach selects and builds a program from the
   modality knowledge base, at the right level (beginner/advanced track),
   possibly combining a primary modality with a maintenance-dose secondary
   (the legal-pairings logic already exists in the knowledge base).
3. **Daily use** — the person logs training, nutrition, sleep. Minimal
   friction is the whole game here — if logging is a chore, the data stops
   coming and the coach goes blind.
4. **Adaptation** — the coach reads the incoming data against the plan and
   adjusts: deload early if recovery markers say so, progress faster if
   everything's easy, flag when a red-flag pattern shows up (the stop-list
   logic already exists in the knowledge base).
5. **Review & re-goal** — at block boundaries (roughly every 12 weeks, same
   cadence as the program library), the coach re-evaluates goals with the
   person and starts the next block.

## Who it's for

**Decided 2026-08-10, corrected same day (see `logs/decisions.md` — the
first pass at this got it backwards):** v0 is **both**, on one system, not
two products:

- **Primary: a public, self-serve B2C app.** Anyone signs up, including
  someone with zero training background, picks (or is matched to) a program
  from the modality library, and is guided through it autonomously — the
  app tells them what to do each session, they log results, the AI coach
  adapts the plan. No human coach is in this loop; the AI *is* the coach.
- **Secondary: an admin/coaching layer for the owner.** The platform owner
  also runs their own real-world coaching practice through the same system
  — an admin role with visibility into and override capability over a
  roster of their own directly-coached clients, layered on top of the same
  program-generation/adaptation engine everyone else uses. Not a
  multi-coach marketplace (other coaches bringing their own client rosters)
  — that's a materially bigger multi-tenancy question, explicitly out of
  scope until raised on purpose.

The two modes share the same knowledge base, data model, and AI
orchestration; the difference is a permissions/visibility layer on specific
client relationships, not a fork of the product.

## Monetization

**Decided 2026-08-10** (see `logs/decisions.md`): tiered, per-modality
unlocks mapped onto the existing 12-week block structure — Free (Wks 1–4),
Tier 1 $18 (Wks 1–8), Tier 2 $45 (full 12 wks + ongoing adaptation), Pro
$200/yr (Tier 2 on every modality). Full logging (nutrition/sleep/weight)
is free regardless of tier — only program depth is paywalled. **Tier
1/Tier 2 purchases are owned forever, no expiry** — buy the $45 unlock
once, that modality is yours permanently on that account. **Pro is the
only tier that expires** — annual, lapses without renewal. See
`domains/data/data-model.md` ("Monetization / entitlements") for the
schema. Still open: whether v0 builds all 26 modality×tier SKUs at once or
starts smaller.

**The one exception: the owner's own personal clients get the entire app
free, no gates at all** — they've already paid the owner directly, offline,
for coaching. They redeem a personal code from the owner during onboarding
instead of paying in-app. See `CoachInviteCode` in `data-model.md`.

## Coaching services & community (new, 2026-08-10)

Beyond the AI-coach product, the owner (certified personal trainer,
currently zero active clients) wants this platform to also be the funnel
and operating system for a real coaching practice:

- **Lead capture** — every signup (including free-tier self-serve users)
  who opts in (`User.marketing_opt_in`) is a prospect for the owner's real
  coaching membership, not just an app user. See `data-model.md`.
- **Community** — coursework/video, live 1:1 or group calls, and a forum
  for paying members. Sketched, not designed — see
  `domains/community/README.md`. Real open questions there: who gets
  access, and whether Base44 natively supports live video/hosting.
- **Human services beyond the AI** — things that need judgment or presence
  the AI can't provide: video form/technique review, event/competition
  prep (several knowledge-base programs already have peaking blocks built
  for this), human-reviewed nutrition, live accountability check-ins.
  Not designed yet, listed here so it isn't lost.

## Not yet decided (flag for a real conversation, not a default)

- **Initial wedge modality — recommended, not yet confirmed:** owner has
  no meaningful existing audience (~340 low-interaction Instagram
  followers, zero clients) and no real combat-sports/Shaolin background
  (an interest, not lived expertise) — but *is* a certified personal
  trainer and a genuine hybrid athlete. Recommendation: lead with
  **Hybrid Training** (`04-hybrid-training.md`) as the launch wedge,
  marketed on real credential + real identity, rather than an audience
  that doesn't exist yet or an interest that isn't expertise yet. Awaiting
  the owner's confirmation before this is logged as decided.
- Native app vs. web app vs. both
- Whether the admin/coaching layer ever opens up to other coaches
  (multi-tenant) — not planned for now, flagged in case it comes up
- SKU build sequencing (all 26 modality×tier combos at once, or fewer to
  start) — tier expiry is resolved, this is just build order
- Community access gating and build sequencing (see
  `domains/community/README.md`)
