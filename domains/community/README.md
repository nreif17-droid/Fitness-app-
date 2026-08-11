# Community Domain (new, 2026-08-10)

## Status: ACTIVE — in scope (reversed 2026-08-10)

**The earlier deferral is reversed.** The owner has asked to proceed with
community, get it hooked up, and make sure it operates properly. Build it.

### ⚠️ Read this first: what's built ≠ what's specced

Assessment of the live app (`logs/apex-vitality-assessment.md`) found that
**community features already exist in Apex Vitality** — but they are a
*different product* from the spec below:

| | **Built in Apex Vitality** | **Specced in this doc** |
|---|---|---|
| Shape | Consumer **social network** | **Coaching community** |
| Entities | `SocialPost`, `Friendship`, `PostLike` | `CourseContent`, `LiveSession`, `LiveSessionAttendee`, `ForumThread`/`ForumPost` |
| Functions | `social-feed`, `social-friends`, `social-leaderboard` | none built |
| UI | `Community.jsx` (15 KB) | none built |
| Who gets in | **Any user.** `SocialPost.visibility` supports `public`; friendships are user-to-user | **Only active `CoachClientRelationship`** clients |
| Purpose | Peer motivation, sharing workouts, leaderboards | Deliver the owner's coaching — courses, live calls, forum |

**These are not the same feature and they don't gate the same way.** The
built version is open to everyone; the specced version is a paid-client
benefit. Both are legitimate; they answer different questions.

**This needs a decision before building further** — see "Access model" below.

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

## Access model — NEEDS A DECISION

The earlier decision (2026-08-10) was: community bundled **exclusively** with an
active `CoachClientRelationship` — not separately purchasable, not tied to app
tier. Drop the coaching relationship, lose community access, but keep comped app
access permanently.

**That decision conflicts with what's actually built.** The live app's social
features are open to all users. So one of these has to give:

| Option | Meaning | Implication |
|---|---|---|
| **A. Two distinct tiers** | Keep the built social feed open to everyone (peer motivation, leaderboards); add the specced coaching community as a *separate, gated* space for active clients | Most work, but both features do what they're each good at. Social drives retention for free users; coaching community delivers paid value |
| **B. Gate everything to clients** | Restrict the existing social features to active `CoachClientRelationship` only | Matches the original decision, but throws away retention value for the ~all of users who aren't the owner's personal clients — and social proof is what makes a young app feel alive |
| **C. Open everything** | Drop the gating entirely; community is a general feature for all users | Simplest; means community stops being a coaching-client benefit, which was its original point |

**Recommendation: A.** They serve genuinely different purposes, and the built
social layer is real retention machinery for exactly the free-tier users the
funnel depends on. Gating it would make a new app feel empty.

**Not decided — needs the owner's call.**

## Open questions (real, not defaulted)

- **Video/live-call infrastructure** — probably needs a third-party
  integration (Zoom/Meet, Vimeo/YouTube-unlisted, or similar) rather than
  being native to what Base44 generates. Unconfirmed — see
  `domains/platform/base44-architecture.md`. The owner previously floated
  that a plain Zoom link may be enough; that remains a live, sensible option
  rather than a fallback.
- **Moderation** — **now urgent, not theoretical.** `SocialPost` already
  supports `visibility: public`, so user-generated content can reach other
  users today. Before real users, this needs at minimum a report path, an
  admin remove capability, and basic community guidelines. Nothing of the
  sort was observed in the assessment.
- **What "operating properly" means** — the built social features were never
  verified end-to-end. Friend requests, feed visibility rules, likes, and the
  leaderboard all need testing against the RLS rules, especially
  `SocialPost.visibility: specific` + `allowed_user_ids`, which is the most
  likely place for a leak.
