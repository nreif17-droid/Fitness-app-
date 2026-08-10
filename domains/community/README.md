# Community Domain (new, 2026-08-10)

## Status: DEFERRED — do not build (decided 2026-08-10)

The spec below is decided and worth keeping accurate, but **it is
explicitly not v0 scope.** Don't implement it, don't prompt Base44 for it,
don't add its entities to a live schema. The owner wants this held as a
documented idea until the core AI-coach loop (self-serve programs +
tracking + adaptation) is live and proven — **surface it again at that
point** rather than waiting to be asked; this is a standing reminder for
whoever (human or Claude) picks this repo up next, not a one-time flag.
See `logs/decisions.md` and `agents/CLAUDE.md` ("Explicitly out of scope
for now").

Not yet designed in depth beyond the spec below — a new idea from the
owner, not part of the original scaffold. See
`domains/data/data-model.md` ("Coaching community") for the sketched
entities (`CourseContent`, `LiveSession`, `LiveSessionAttendee`,
`ForumThread`/`ForumPost`) — sketched for reference, not for building yet.

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
- **Sequencing** — **decided: deferred, not v0** (see Status above). Revisit
  once the core loop is proven.
- **Build location** — when this is revisited, don't assume in-app is the
  answer. The owner explicitly floated running this on a separate platform
  or tool instead (a plain Zoom link for live calls, a separate community
  app/product) rather than building community infrastructure into this
  platform. That's a live option, not a fallback of last resort.
