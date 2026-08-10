# Data Model (draft)

This is a starting schema, not a final one — treat it as the thing to argue
with once real screens get built in Base44. Every entity below assumes a
`user_id` foreign key and standard `created_at`/`updated_at`.

## Identity & profile

**User**
`id, name, email, auth_provider, timezone, units_preference (imperial/metric), created_at`

**AthleteProfile**
`user_id, age, height, bodyweight_current, training_age_years, sex_for_physiology (distinct from identity fields — used only for load/heart-rate/nutrition physiology, never surfaced as a label), injury_history (free text + structured flags), equipment_access (home/gym/outdoor), primary_modality, secondary_modality`

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

- **New, from the coach-facing decision:** this schema still assumes `User`
  is the athlete. A coach-facing v0 needs a `Coach` entity and some kind of
  `CoachClientRelationship` (coach_id, client_user_id, status) so a coach can
  see/manage multiple `AthleteProfile`s, and permissions on who can read/
  write a given client's data. Not modeled yet — flagging rather than
  guessing at the shape before the coach-facing UX is sketched.
- Whether ProgramInstance supports true hybrid/custom programs the coach
  assembles dynamically, vs. only ever assigning one of the fixed templates.
  The knowledge base is written as fixed 12-week templates; a coach that
  actually personalizes will eventually need to compose novel sessions from
  the underlying principles (Part 0 framework), not just pick a template off
  the shelf — that's a meaningfully bigger AI-orchestration problem than
  template selection, worth being honest about scoping into a later phase.
