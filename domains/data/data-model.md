# Data Model (draft)

This is a starting schema, not a final one — treat it as the thing to argue
with once real screens get built in Base44. Every entity below assumes a
`user_id` foreign key and standard `created_at`/`updated_at`.

## Identity & profile

**User**
`id, name, email, auth_provider, timezone, units_preference (imperial/metric), created_at`

**AthleteProfile**
`user_id, age, height, bodyweight_current, training_age_years, sex_for_physiology (distinct from identity fields — used only for load/heart-rate/nutrition physiology, never surfaced as a label), injury_history (free text + structured flags), equipment_access (home/gym/outdoor), primary_modality, secondary_modality`

## Coaching relationship (admin layer)

**Decided 2026-08-10** (see `logs/decisions.md`): v0 is primarily self-serve
B2C — most `User` rows have no coach and are run entirely by the AI. On top
of that, the platform owner also runs their own coaching practice through
this same system. This is modeled as a relationship on specific users, not
a fork of the schema:

`User.role` — `member` (default) or `admin`. Single admin (the owner) for
v0, not a multi-tenant coach roster — see open questions below.

**CoachClientRelationship**
`id, admin_user_id, client_user_id, status (active/paused/ended), started_at, notes`

A row here is what turns a normal self-serve `User` into one of the owner's
directly-coached clients: it grants the admin read access to that client's
`ProgramInstance`, `Session`/`SessionLog`, `ReadinessCheckIn`, and
`CoachDecisionLog`, and write/override access to adjust their program. No
row = pure self-serve, AI-only, admin has no special access. Everything
else in this schema (programs, logs, coach memory) is identical either way
— this table is the only thing that distinguishes the two modes.

**Decided 2026-08-10** (see `logs/decisions.md`): the AI acts autonomously
for coached clients exactly as it does for self-serve users — **no
approval gate**, admin visibility/override is after-the-fact via
`CoachDecisionLog`, not a review step blocking what the client sees. The
one thing a `CoachClientRelationship` adds beyond visibility is a direct
line to the admin:

**CoachMessage**
`id, coach_client_relationship_id, sender (client/admin), body, sent_at, read_at`

A simple in-app thread between a coached client and the admin, available
alongside (not instead of) full AI access. Only exists where a
`CoachClientRelationship` exists — self-serve users have no one to message.

**Decided 2026-08-10** (see `logs/decisions.md`): personal clients — people
who paid the admin directly, offline, for coaching — get **the entire app
free**, no tier gates at all. They redeem a code during onboarding rather
than the admin manually flipping a flag per user:

**CoachInviteCode**
`id, admin_user_id, code (unique string), max_redemptions (default 1), redemption_count, status (active/revoked), created_at, notes`

**CoachInviteCodeRedemption**
`id, invite_code_id, redeemed_by_user_id, redeemed_at`

Entering a valid code during onboarding does two things atomically: (1)
creates a `CoachClientRelationship(admin_user_id, client_user_id, status:
active)`, and (2) grants an `Entitlement(user_id, modality: null, tier:
pro, price_paid_cents: 0, source: admin_grant)` — full Pro-equivalent
access, every modality, no purchase. `max_redemptions` defaults to 1 (one
code per client) so a leaked code can't be reused broadly; the admin can
raise it deliberately for a code meant to be shared.

**Open, not yet decided:** if a `CoachClientRelationship` later ends
(`status: ended` — the person stops being a coaching client), does the
comp'd `Entitlement` from decision get revoked, or does it persist
permanently since "they already paid"? Worth a real answer before this
matters in practice, not a default.

## Goals

**Goal**
`id, user_id, description, target_metric, target_value, target_date, status (active/achieved/abandoned/superseded), created_from (onboarding/re-goal/manual)`

Goals are the thing the AI coach reasons backward from when selecting a
program — see `domains/ai-coach/orchestration.md`.

## Programs (the knowledge base made instantiable)

**ProgramTemplate** — the static library, one row per program in
`domains/training/knowledge-base/`
`id, modality, level (beginner/advanced), name, source_file, weeks (12), description`

**ProgramInstance** — a template assigned to a specific user
`id, user_id, template_id, secondary_template_id (nullable — maintenance-dose pairing), start_date, current_week, status (active/completed/abandoned), adaptations_log (JSON — every deviation from the template and why)`

## Monetization / entitlements

**Decided 2026-08-10** (see `logs/decisions.md`): tiered, per-modality
unlocks, mapped directly onto the block boundaries every program in the
knowledge base already uses (`00-framework.md` — Accumulate Wks 1–4,
Intensify Wks 5–8, Realize/Taper Wks 9–12). This is a system-wide rule off
Part 0, not a per-template field:

| Tier | Price | Unlocks | Depth |
|---|---|---|---|
| Free | — | Weeks 1–4 of any modality | Basic — gets someone started |
| Tier 1 | $18 / modality | Weeks 1–8 | More refined, more escalating |
| Tier 2 | $45 / modality | Full 12 weeks + ongoing adaptation | Fully tailored, continues past Wk 12 |
| Pro | $200 / yr | Tier 2 depth on **all** modalities | Everything |

Free-tier tracking (nutrition/sleep/weight logging) is **not** gated —
every user gets full logging regardless of tier; only program depth is
paywalled.

**Entitlement**
`id, user_id, modality (nullable — null means Pro, all modalities), tier (free/tier_1/tier_2/pro), granted_at, price_paid_cents, expires_at (nullable), source (purchase/comp/admin_grant)`

A user with no `Entitlement` row for a modality still gets the free tier by
default (no row required). `ProgramInstance`/`Session` generation checks
the relevant `Entitlement.tier` to decide how many weeks to reveal and
whether adaptation continues past Week 12.

**Open, not yet decided (see `logs/decisions.md`):**
- Does Tier 1/Tier 2 access expire, or is it owned permanently once
  purchased (including ongoing post-Wk-12 adaptation on Tier 2)? This is
  what `expires_at` is for, but whether it's ever populated is undecided.
- Whether v0 actually builds all 13 modalities × Tier 1/Tier 2 (26 SKUs)
  plus Pro, or starts with less granularity and expands — the tier
  *concept* is decided, this is about build sequencing.

**Session** (planned) — one prescribed workout, generated from a
ProgramInstance for a specific date
`id, program_instance_id, week_number, day_label, modality, prescription (JSON — the sets/reps/pace/load actually assigned), status (upcoming/completed/skipped/modified)`

**SessionLog** (actual) — what really happened
`id, session_id, user_id, completed_at, actual_prescription (JSON), rpe_reported, notes, duration_minutes`

## Nutrition

**NutritionLog**
`id, user_id, date, calories, protein_g, carbs_g, fat_g, source (manual/photo/barcode/integration), notes`

**NutritionTarget**
`id, user_id, effective_date, calories, protein_g, carbs_g, fat_g, rationale (linked to current program phase — bulk/cut/maintain/endurance-fueling)`

## Sleep

**SleepLog**
`id, user_id, date, hours, quality_score (1-10 self-report, or device-sourced), source (manual/wearable-integration)`

## Body & readiness metrics

**BodyMetric**
`id, user_id, date, metric_type (weight/bodyfat/tape-measurement/photo), value, unit`

**ReadinessCheckIn**
`id, user_id, date, readiness_score (1-10, pre-training self-report), resting_hr, hrv (if integrated), soreness_flags, notes`

This is the table the coach's autoregulation logic (deload-early triggers,
the "two nights of poor sleep or RHR +7bpm" rule already codified in the
knowledge base) actually reads from.

## Test / benchmark results

**BenchmarkResult**
`id, user_id, modality, test_type (e.g. "5K time trial", "1RM squat", "FTP test", "CSS"), value, unit, date, context (week_0/week_12/ad_hoc)`

This is what closes the loop — Week 0 vs. Week 12 comparisons per the
knowledge base's testing protocol live here.

## AI coach memory

**CoachMemory** — durable facts the coach has learned about this specific
user, **strictly per-user, no population-level layer** (decided
2026-08-10, see `logs/decisions.md` — this is the per-user equivalent of
the memory file convention already in use elsewhere)
`id, user_id, category (constraint/preference/pattern/injury-flag), content, source_session_id, confidence, superseded_by (nullable)`

**CoachDecisionLog** — every material adjustment the coach made and why,
for auditability and for the user to see "why did my program change"
`id, user_id, program_instance_id, date, decision, trigger_data, rationale_summary`

## Open questions this schema doesn't resolve yet

- Whether the admin/coaching layer ever needs to support more than one
  admin (other coaches bringing their own client rosters, i.e. true
  multi-tenancy) — not planned for v0 (`CoachClientRelationship.admin_user_id`
  is written generically enough to extend later, but nothing above assumes
  more than one admin exists).
- Whether ProgramInstance supports true hybrid/custom programs the coach
  assembles dynamically, vs. only ever assigning one of the fixed templates.
  The knowledge base is written as fixed 12-week templates; a coach that
  actually personalizes will eventually need to compose novel sessions from
  the underlying principles (Part 0 framework), not just pick a template off
  the shelf — that's a meaningfully bigger AI-orchestration problem than
  template selection, worth being honest about scoping into a later phase.
