# Community Domain (new, 2026-08-10)

Not yet designed in depth — a new idea from the owner, not part of the
original scaffold. Placeholder scope, see `logs/decisions.md` for how this
came up and `domains/data/data-model.md` ("Coaching community") for the
sketched entities (`CourseContent`, `LiveSession`, `LiveSessionAttendee`,
`ForumThread`/`ForumPost`).

## The idea

On top of the AI-coach product (programming, adaptation, tracking), give
the owner's coaching practice a place to run the parts of coaching that
need a human presence or a group, not just an individual adaptive plan:

- **Coursework / video library** — educational content beyond what a
  program prescription communicates (technique breakdowns, philosophy,
  Q&A recordings).
- **Live calls** — 1:1 or group, scheduled, for the kind of real-time
  coaching a static program and async messaging (`CoachMessage`) can't
  replace.
- **Forum** — a space for paying members to interact with each other, not
  just with the AI or the admin. Community-as-retention, not just
  content-as-retention.

## Access (decided 2026-08-10)

Bundled exclusively with an **active** `CoachClientRelationship` — not a
separately purchasable membership in v0, and not tied to general app tier
(a Pro subscriber who isn't the admin's personal client doesn't get it).
Dropping the coaching relationship removes community access immediately,
even though the person keeps whatever general app access they were
comped, permanently — deliberate split: general app access is "yours
because you were given it," community/direct access is "yours because
you're actively my client."

## Open questions (real, not defaulted)

- **Video/live-call infrastructure** — probably needs a third-party
  integration (Zoom/Meet, Vimeo/YouTube-unlisted, or similar) rather than
  being native to what Base44 generates. Unconfirmed — see
  `domains/platform/base44-architecture.md`.
- **Moderation** — a forum needs at least a report/remove path and basic
  community guidelines before it's live with real users, even at small
  scale. Not designed yet.
- **Sequencing** — this is meaningfully more scope than the core
  AI-coach product. Worth deciding whether it's v0 or a fast-follow once
  the core loop (self-serve programs + tracking + adaptation) is proven,
  rather than building both at once.
