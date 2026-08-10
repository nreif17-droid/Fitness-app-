# Decision Log

Append-only. Each entry: date, decision, reasoning. Update the doc that
originally flagged the question as open once it's answered here.

---

### 2026-08-10 — Initial scaffold created

Repo structure, vision draft, data model draft, AI coach orchestration
draft, and the 16-part training knowledge base (13 modalities +
framework + usage guide) assembled from prior work. Named `fitness-platform`
as a placeholder — rename before this goes live if a real brand name lands.

Open questions deliberately left unresolved rather than defaulted, flagged
in place across `vision/product-vision.md`, `domains/data/data-model.md`,
and `domains/ai-coach/orchestration.md`:
- Target user / launch wedge
- B2C vs. coach-facing tool vs. both
- Monetization
- Whether "collective consciousness" means real cross-user learning
- Base44 GitHub sync semantics, exported stack, and plan-gating — needs
  verification against current docs, not assumption

---

### 2026-08-10 — Target user and coach-memory scope decided

**Decision 1: v0 target user is a coach-facing tool**, not direct B2C.
Built for a coaching practice (e.g. the Sedona coaching concept) to run
multiple clients through, not a self-serve signup product. A B2C layer on
top isn't ruled out long-term but isn't v0.
Reasoning: user's explicit call when asked directly. Resolves the
"target user / launch wedge" and "B2C vs. coach-facing" questions raised
in the initial scaffold.
Updated: `vision/product-vision.md` ("Who it's for" / "Not yet decided").
**New follow-on, not yet resolved:** the data model and orchestration docs
both assumed a single end-user; a coach-facing product needs a `Coach`
entity, a coach-client relationship, and a decision on who the AI is
actually conversing with at each step (coach on client's behalf vs. client
directly). Flagged in `data-model.md` and `orchestration.md`, not designed.

**Decision 2: AI coach memory is per-user only.** No population-level /
cross-user "collective consciousness" learning layer in `CoachMemory`, now
or in any near-term phase — this is a firm decision, not just a v0
deferral, made explicitly for privacy/consent simplicity.
Reasoning: user's explicit call when asked directly.
Updated: `domains/data/data-model.md` (CoachMemory definition + open
questions), `domains/ai-coach/orchestration.md` ("Not yet designed"),
`agents/CLAUDE.md` (out-of-scope section).

Still open: initial wedge modality, monetization, native vs. web, and
Base44 GitHub sync/export/plan-gating verification.

---

### 2026-08-10 — Correction: target user is B2C self-serve + owner's own coaching layer, not coach-facing-only

**Supersedes Decision 1 above** (same day) — that entry was wrong. The
options presented in the prior AskUserQuestion collapsed "coach-facing"
into a single choice; the user's actual intent, given directly afterward,
is both, on one system:

- **Primary: public B2C, self-serve.** Anyone (including someone with no
  training background) signs up, picks or is matched to a program, and is
  guided through it autonomously by the AI coach with no human coach
  involved. This is the main product.
- **Secondary: the owner also runs their own coaching practice through the
  same platform**, as admin — visibility into and override capability over
  a roster of their own directly-coached clients.
- **Not** a multi-coach marketplace — single admin (the owner) for v0,
  explicitly out of scope to build for multiple coaches/tenants.

Reasoning: user's direct correction, unprompted, on re-reading the earlier
decision back to them — a case for asking rather than assuming when a
generated option list may not cover the actual shape of what someone wants.

Updated: `vision/product-vision.md` ("Who it's for" / "Not yet decided"),
`domains/data/data-model.md` (new "Coaching relationship (admin layer)"
section: `User.role`, `CoachClientRelationship`; open questions revised),
`domains/ai-coach/orchestration.md` ("Not yet designed" — admin-coached
path), `agents/CLAUDE.md` (out-of-scope: multi-tenant coaching named
explicitly).

Decision 2 (per-user coach memory only) is unaffected — it's about
cross-user learning, not about who's a user vs. an admin, and holds
regardless of which target-user model is correct.

Still open: initial wedge modality; monetization (and whether it differs
between self-serve and coached users); native vs. web; whether the
admin/coaching layer ever opens to other coaches; exact AI/admin
permission boundary on a coached client; Base44 GitHub sync/export/
plan-gating verification.

---

### 2026-08-10 — Monetization model and admin-oversight boundary decided

**Decision 3: monetization is tiered, per-modality unlocks**, mapped onto
the 12-week block structure already in the knowledge base:
- **Free** — Weeks 1–4 of any modality (Accumulate block). Basic, gets
  someone started. Full logging (nutrition/sleep/weight) is free regardless
  of tier.
- **Tier 1 — $18/modality** — Weeks 1–8 (Accumulate + Intensify). More
  refined, escalating.
- **Tier 2 — $45/modality** — full 12 weeks, fully tailored, continues
  adapting past Week 12.
- **Pro — $200/yr** — Tier 2 depth on every modality.
Reasoning: user's explicit design, volunteered unprompted. Maps cleanly
onto content that already exists (Part 0's Accumulate/Intensify/Realize
boundaries), so it's a paywall on existing content, not a content rewrite.
Updated: `vision/product-vision.md` (new "Monetization" section),
`domains/data/data-model.md` (new "Monetization / entitlements" section:
`Entitlement`).
**Still open:** whether Tier 1/Tier 2 access ever expires (`expires_at` is
modeled but not decided), and whether v0 builds all 26 modality×tier SKUs
at once or starts smaller.

**Decision 4: no approval gate for admin-coached clients.** The AI acts
identically for self-serve and coached users — same autonomy, no
"admin reviews before the client sees it" step. The admin's
`CoachClientRelationship` adds after-the-fact visibility/override
(`CoachDecisionLog`) plus a **direct in-app messaging channel** to the
admin, available alongside full AI access, not instead of it.
Reasoning: user's explicit call when asked directly.
Updated: `domains/ai-coach/orchestration.md` ("Not yet designed" — admin-
coached path, now resolved), `domains/data/data-model.md` (new
`CoachMessage` entity, removed the now-resolved permission-boundary open
question).

Still open: initial wedge modality (actively being explored — the owner's
own background/audience is a live input, not yet landed); native vs. web;
multi-tenant coaching (not planned); tier expiry semantics and SKU build
sequencing; Base44 GitHub sync/export/plan-gating verification.
