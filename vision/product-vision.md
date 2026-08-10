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

**Decided 2026-08-10 (see `logs/decisions.md`): a coach-facing tool.** v0 is
built for a coaching practice (e.g. the Sedona coaching concept) to run
multiple clients through, not a direct-to-consumer self-serve app. This
means the primary user of the onboarding flow, the dashboards, and the
"why did the program change" visibility is the coach acting on a client's
behalf (or the coach reviewing/approving what the AI proposed for a
client), not an anonymous general-population signup. Revisit whether a
B2C self-serve layer gets added on top once the coach-facing tool is
proven — it isn't ruled out, just not v0.

## Not yet decided (flag for a real conversation, not a default)

- Initial wedge modality to launch with (which of the 13 in the knowledge
  base the coaching practice actually runs first)
- Monetization model
- Native app vs. web app vs. both
