# Apex Vitality — Existing App Assessment

**Date:** 2026-08-10
**App:** Apex Vitality (Base44 app id `6a78de5f6a884d5cd8b7a267`)
**Method:** direct read of entity schemas, backend functions, and frontend source via
the Base44 MCP server.
**Scope caveat:** the MCP connection dropped partway through. I read the entity
schemas, the full directory tree, `generate-program/entry.ts`, `ProGate.jsx`,
`AGENTS.md`, and `CLAUDE.md`. I did **not** get to `Training.jsx`, `Programs.jsx`,
`user-context.ts`, the payments functions, or `recovery-insight`. Findings below are
solid for what was read; the unread files could change some conclusions.

---

## Headline

The app is **substantially better built than the "AI slop" worry implies**, and the
quality problem is **not** diffuse. It traces to one architectural decision in one
file. That's good news: it's fixable without a rebuild.

---

## What's genuinely good (don't rebuild these)

1. **Row-Level Security is correctly implemented.** Every user-data entity carries
   `created_by_id: {{user.id}}` checks on read/update/delete with an admin override.
   This is the single most common catastrophic failure in AI-generated apps — the
   thing the build playbook warns about hardest — and it was done right. Real, load-
   bearing quality.
2. **Sound modern stack and structure.** React + Vite + Tailwind, 17 pages, shadcn-style
   UI components, sensible `src/lib` separation, real auth flows (login, register,
   reset, OAuth consent), protected routes.
3. **14 backend functions** including payments webhook, TDEE engine, meal analysis,
   wearable import, voice logging, transcription. This is a real application, not a demo.
4. **The universal framework IS in the codebase.** `generate-program/entry.ts` contains
   a `FRAMEWORK` constant that faithfully condenses Part 0 of the training library —
   the three blocks, deload rules, progression levers, autoregulation, 80/20, the
   non-negotiables, and the red-flag stop list. So the app is *not* improvising sports
   science from nothing. That instinct was already right.
5. **All 16 modalities** are present in `WorkoutProgram.athlete_type`.

---

## The core problem: one LLM call generates the entire 12-week program

`base44/functions/generate-program/entry.ts` asks a single `InvokeLLM` call to produce
**`weeks × days_per_week` day entries — 48 to 60 days** — each with a warmup,
multiple `main_blocks` (name, sets, reps, rest, target, instructions), cooldown,
hourly plan, and mentality cue. In one response. With no validation of what comes back.

**This is the quality ceiling you're feeling.** Five consequences:

### 1. Output degradation — and it hits the paid weeks hardest
A 60-day program with ~6 exercises per day is an enormous JSON payload. Models
predictably rush, thin out, or truncate as generation length grows. **Weeks 1–3 will
read well and Weeks 9–12 will be noticeably weaker** — which is exactly backwards from
what the monetization model needs, since those late weeks are what people pay for.

### 2. Nothing verifies the framework was followed
The framework is *described* to the model, never *enforced*. Nothing checks that Week 4
volume actually dropped 40%. Nothing checks Week 8 deloaded. Nothing checks progressive
overload happened at all, or that Week 12 is a taper. **The periodization is a
suggestion in a prompt, not a property of the output.** If the model drifts, no one
finds out.

### 3. The library's actual week-by-week prescriptions are being thrown away
`WorkoutProgram.days[]` has **no `week` field** — the stored template is a *single base
week*, plus a free-text `progression` string. The generator then asks the LLM to
"periodize this base week across 12 weeks."

But the knowledge base already contains literal, specific, week-by-week tables —
powerlifting Week 5 is `5×4 @ 77.5%`, running Week 7 is `4 × (6 min run / 90 s walk)`,
the erg build has a full 12-week ladder. **That real content is not reaching the app.
It's being replaced by plausible-sounding invention.** This is the single biggest
reason output feels generic: the app is asking a model to re-derive what you already
wrote down precisely.

### 4. Non-deterministic and unreproducible
Same athlete, same program, two taps → two different programs. No way to reproduce, diff,
review, or fix a specific bad output.

### 5. Only the framework got in — none of the modality knowledge
`FRAMEWORK` is ~15 lines. The 13 modality files, and the nutrition/sleep/recovery/
adherence knowledge bases added today, are **entirely absent from the app.** The
`templateSummary` passed to the model is just the base week's exercise names.

---

## Confirmed bugs and gaps

### 🔴 Entitlement bypass — free users can get paid programs
`generate-program/entry.ts` takes a `programId`, looks up the program, and generates
the full program. **There is no entitlement check anywhere in the function.**
Meanwhile `WorkoutProgram`'s RLS read rule is `{}` — fully open — so every program row,
premium included, is readable by any authenticated user.

`ProGate.jsx` is a **purely client-side** component: it takes an `isPro` boolean and
conditionally renders JSX. That is UI concealment, not access control.

**Net effect: any logged-in user can invoke `generate-program` with any premium
program's id and receive the complete paid program for free.** This is precisely the
"can a free user reach Week 9 content by manipulating a request" item from the build
playbook's checklist — present and confirmed. It needs a server-side entitlement check
before generation, and tighter RLS on premium program bodies.

### 🔴 Autoregulation cannot function — the data isn't collected
The FRAMEWORK text instructs: *"If prescribed load feels 2+ RPE harder than target for
two sessions running, take the deload early."*

But **there is no RPE or RIR field anywhere in the schema.** `main_blocks` has
sets/reps/rest_sec/target/instructions; `WorkoutSession.exercises` has
name/sets/reps/weight/notes. There is no `ReadinessCheckIn` entity either.

So the app *describes* autoregulation to the model and then never collects the inputs
that would let it happen. **The adaptive coaching — the entire product differentiator —
has no data to adapt on.** `SleepRecord` does capture `hrv_avg`, which is the least
reliable signal of the set (see `domains/sleep/knowledge-base/01-recovery-and-readiness.md`).

### 🟠 Monetization model doesn't match what was decided
Built: `UserProfile.plan_tier: free|pro` plus `owned_program_ids[]`, and
`WorkoutProgram.price_usd` / `is_premium` — i.e. per-program purchase plus a binary Pro.

Decided (see `logs/decisions.md`): Free = Weeks 1–4 · $18 = Weeks 1–8 · $45 = full 12
weeks + ongoing · $200/yr Pro = everything.

**The week-gating that the whole tier model depends on does not exist**, in the schema
or in the generator. This is the biggest spec-vs-build divergence.

### 🟠 The admin/coaching layer doesn't exist
No `CoachClientRelationship`, no `CoachInviteCode`, no `CoachMessage`, no
`CoachDecisionLog`. `User.role` does have `admin`, and RLS respects it, so the
foundation is there — but the coaching-practice layer that was specced is unbuilt.

### 🟠 Community is built, but was decided as deferred
`SocialPost`, `Friendship`, `PostLike`, `Community.jsx` (15 KB), plus `social-feed`,
`social-friends`, and `social-leaderboard` functions all exist. The repo decision was
to **defer** community. Not wrong — just a live conflict to resolve deliberately:
either the decision changes, or this ships dormant.

### 🟡 Nutrition targets are hardcoded, not derived
`UserProfile` defaults to `daily_calorie_target: 2200`, `protein_target_g: 150`,
`carb_target_g: 220`, `fat_target_g: 70`. A `tdee-engine` function exists (unread), but
the defaults aren't derived from bodyweight, goal, or training phase as
`domains/nutrition/knowledge-base/00-framework.md` specifies.

### 🟡 Enum drift between entities
`WorkoutProgram.athlete_type` has all 16 modalities. `UserProfile.training_type` is
missing six of them (mobility, rowing, rucking, combat, olympic_weightlifting,
strongman). A user cannot describe themselves as the thing the catalog sells.

### 🟡 Scope-of-practice exposure is live, not theoretical
The app already generates calorie and macro targets and runs meal analysis/suggestion.
The open legal question in `domains/nutrition/knowledge-base/03-safety-and-scope.md`
is not hypothetical — it is shipping behavior today. No health-screening or refusal
path was observed.

---

## Recommended fix sequence

Ordered by value-per-effort. None of this requires a rebuild.

**1. Close the entitlement bypass.** Server-side check in `generate-program` before
generating; tighten `WorkoutProgram` RLS so premium bodies aren't world-readable.
Small change, directly protects revenue.

**2. Add RPE/RIR capture + a readiness check-in.** Add `rpe` to logged sets and a
lightweight daily `ReadinessCheckIn`. Without this the coach is blind, and no amount of
prompt improvement fixes it. This unblocks the actual differentiator.

**3. Replace one-shot generation with per-week generation, validated.** The highest-
leverage change:
   - Add a `week` dimension to `WorkoutProgram.days` and store the library's **real**
     week-by-week prescriptions instead of a single base week.
   - Generate (or better, assemble deterministically) **one week at a time**, not 60
     days at once.
   - **Validate after generation**: assert Week 4 and Week 8 volume actually dropped,
     Week 12 is a taper, load progresses. Reject and regenerate on failure.
   - Blocks, deloads, and week-gating are deterministic — they should be code, not
     prompt suggestions. Reserve the LLM for personalization *within* validated
     structure. (Build playbook §2.)

**4. Get the modality knowledge into the app.** The per-modality files are the actual
IP. Retrieve the relevant one into context per generation rather than relying on a
15-line framework summary.

**5. Then the spec divergences:** week-based tier gating, the coaching layer, enum
alignment, derived nutrition targets — and a real decision on the community conflict
and the nutrition scope question.

---

## The honest answer to "is this the best I'm going to get?"

**No.** What was observed is not a Base44 capability ceiling. Base44 did the parts it's
good at genuinely well — auth, RLS, CRUD, structure, payments plumbing. The weakness is
concentrated exactly where the build playbook predicted: **the one place where complex,
correctness-critical logic got delegated to a single unvalidated LLM call.**

That is a fixable architectural decision, not a platform limit — and fixing it doesn't
mean abandoning the work already done.
