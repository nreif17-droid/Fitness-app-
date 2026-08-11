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

---

### 2026-08-10 — Personal clients get full free access via invite code

**Decision 5:** anyone who becomes the owner's personal (offline-paid)
coaching client gets the **entire app free — every modality, every tier,
no gates** — because they already paid the owner directly for coaching.
They redeem a code from the owner during onboarding rather than the admin
manually granting access per user.
Reasoning: user's explicit design, volunteered unprompted — the initial
"40% off with a code" idea was revised mid-message to "free, full access,
no gates" and that's the version implemented.
Updated: `domains/data/data-model.md` (new `CoachInviteCode` and
`CoachInviteCodeRedemption` entities — redemption creates a
`CoachClientRelationship` and grants a comped `Entitlement` in one step),
`vision/product-vision.md` (Monetization section, the free-for-personal-
clients exception), `domains/ai-coach/orchestration.md` (code redemption
placed at the front of the intake job).

**New open question this raises, not resolved:** if a `CoachClientRelationship`
later ends, does the comped `Entitlement` get revoked or persist
permanently ("they already paid")? Flagged in `data-model.md`, needs a
real answer before it matters in practice.

Still open: initial wedge modality; native vs. web; multi-tenant coaching
(not planned); tier expiry semantics and SKU build sequencing; whether a
comped entitlement survives the coaching relationship ending; Base44
GitHub sync/export/plan-gating verification.

---

### 2026-08-10 — Tier expiry resolved; wedge context gathered (still open)

**Decision 6: Tier 1 ($18) and Tier 2 ($45) per-modality unlocks never
expire.** Buy once, own that modality's unlocked depth on that account
permanently — including Tier 2's ongoing post-Wk-12 adaptation. **Pro
($200/yr) is the only tier that expires**, annually, per its own pricing
(this was already implied by "/yr" and is now explicit). This resolves the
`Entitlement.expires_at` open question for purchased tiers.
Reasoning: user's explicit call when asked directly.
Updated: `domains/data/data-model.md` (Monetization table + Entitlement
section), `vision/product-vision.md` (Monetization section).
Still genuinely open: whether a *comped* entitlement (from `CoachInviteCode`
redemption) behaves like permanent Tier 1/2 or like expiring Pro if the
underlying `CoachClientRelationship` ends — not the same question, not
resolved by this decision.

**Wedge context, not yet a decision:** owner currently has no meaningful
audience to leverage — ~340 Instagram followers from "back home" with low
interaction, zero coaching clients. This rules out an audience-arbitrage
wedge (there's no existing audience in any modality to point at) and means
the free-tier-as-market-test option, or a wedge grounded in the owner's own
personal training background/expertise, are the live paths. Personal
background (e.g. the "Shaolin discipleship track" referenced in the
knowledge base) not yet confirmed by the owner — question still open.

Still open: initial wedge modality; native vs. web; multi-tenant coaching
(not planned); SKU build sequencing; whether comped entitlements survive
the coaching relationship ending; Base44 GitHub sync/export/plan-gating
verification.

---

### 2026-08-10 — Real background clarified; lead capture decided; community proposed

**Background clarified (not combat sports):** owner's real background is
**certified personal trainer + genuine hybrid athlete**; combat
sports/Shaolin is an interest, not lived expertise. Recommendation given
to lead the launch wedge with **Hybrid Training** (`04-hybrid-training.md`)
on that real credential/identity — logged as a recommendation in
`vision/product-vision.md`, not yet confirmed by the owner, so **not**
counted as a resolved decision.

**Decision 7: lead capture for the owner's real coaching practice.** Every
signup, including free-tier self-serve users, is a prospect. Added
`User.marketing_opt_in` (explicit opt-in, not defaulted true — compliance
requirement, flagged as such) so the admin can build an outreach list.
Reasoning: user's explicit, unambiguous ask.
Updated: `domains/data/data-model.md` (User fields).

**New idea, not yet a decision: coaching community** — coursework/video,
live 1:1/group calls, forum, for the owner's paying coaching clients.
Sketched (not designed) in a new `domains/community/README.md` and in
`data-model.md` (`CourseContent`, `LiveSession`, `LiveSessionAttendee`,
`ForumThread`/`ForumPost`). Two real open questions raised, not resolved:
whether access is bundled with `CoachClientRelationship` specifically or a
separate purchasable membership, and whether Base44 natively supports live
video/hosting (flagged in `domains/platform/base44-architecture.md`
alongside its other unverified platform questions).
Updated: `README.md` (folder structure), `domains/ai-coach/orchestration.md`
(new "Adjacent but not an AI job" section — outreach/community are
human-run, not part of the AI coach's three jobs).

Still open: initial wedge modality (recommendation given, awaiting
confirmation); native vs. web; multi-tenant coaching (not planned); SKU
build sequencing; whether comped entitlements survive the coaching
relationship ending; community access gating; community/live-video
technical feasibility on Base44; Base44 GitHub sync/export/plan-gating
verification.

---

### 2026-08-10 — Comped entitlement persistence and community access gating resolved

**Decision 8: comped app access is permanent; relationship-specific access
is not.** If a `CoachClientRelationship` ends (client drops the coaching
membership), the comped `Entitlement` from `CoachInviteCode` redemption
**persists permanently** — same rule as a purchased Tier 1/2 unlock, "you
own what you were given." What ends immediately with the relationship is
everything actually tied to it: `CoachMessage` (direct access to the
admin) and community access — both now explicitly gated on
`CoachClientRelationship.status: active`, not on general app tier.
Reasoning: user's explicit call when asked directly.

**This also resolves the community access-gating open question**
(originally raised in the "personal-client free access" and "coaching
community" entries above): community is **exclusively** a benefit of an
active coaching relationship, not a separately purchasable membership —
so there is no self-serve/Pro path to community access in v0.

Updated: `domains/data/data-model.md` (`CoachMessage`, `CoachInviteCode`
open-question resolved, "Coaching community" access section),
`domains/community/README.md` (new "Access" section replacing the old open
question), `vision/product-vision.md` (Monetization + Coaching services &
community sections).

**Also reconfirmed (not new):** owner has no Shaolin/combat-sports
background — consistent with what was said earlier, strengthens rather
than changes the Hybrid Training wedge recommendation, which is still
awaiting an explicit yes/no.

Still open: initial wedge modality (recommendation given, awaiting
confirmation); native vs. web; multi-tenant coaching (not planned); SKU
build sequencing; community build sequencing and Base44 live-video/hosting
feasibility; Base44 GitHub sync/export/plan-gating verification.

---

### 2026-08-10 — Wedge confirmed; community explicitly deferred; opt-in UX confirmed

**Wedge: confirmed.** Owner said yes to the Hybrid Training recommendation
— **launch wedge is Hybrid Training** (`04-hybrid-training.md`), marketed
on the owner's real CPT credential and genuine hybrid-athlete identity.
Promoted from recommendation to decided; `vision/product-vision.md` now
has a dedicated "Launch wedge" section instead of a "Not yet decided"
bullet.

**Decision 9: community is explicitly deferred, not cut.** The full spec
(coursework/video, live calls, forum, access gating) stays decided and
documented, but must **not** be built, prompted to Base44, or added to a
live schema yet. Hold it as an idea until the core AI-coach loop (self-
serve programs + tracking + adaptation) is live and proven, then surface
it again proactively — a standing reminder for whoever works this repo
next, not a one-time flag. Owner is also open to *not* building it in-app
at all when the time comes — a plain Zoom link or a separate community
platform/app are live alternatives to building infrastructure here, not
fallbacks of last resort. Reasoning: user's explicit call.
Updated: `agents/CLAUDE.md` ("Explicitly out of scope for now" — standing
reminder, also cleaned up a stale line that still said monetization was
undecided), `domains/community/README.md` (new "Status: DEFERRED" header),
`domains/data/data-model.md` (community section marked deferred, not a
build target), `vision/product-vision.md` (community bullet marked
deferred).

**Confirmed, not new:** email opt-in UX is a plain toggle at signup, off
by default — matches what was already modeled for `marketing_opt_in`,
just made explicit in `data-model.md`.

Still open: native vs. web; multi-tenant coaching (not planned); SKU build
sequencing; community build sequencing and Base44 live-video/hosting
feasibility (both dormant until community is revisited); Base44 GitHub
sync/export/plan-gating verification.

---

### 2026-08-10 — Research expansion: nutrition, sleep/recovery, adherence, Base44 build strategy

**Not a decision — a substantial knowledge-base expansion** requested by the owner
("maximum launch research"), plus research into getting quality output from Base44.

**Added, with sources cited inline:**
- `domains/nutrition/knowledge-base/` (4 files) — the domain was a 16-line stub despite
  being named in the product pitch. Now covers energy balance, protein, body
  composition rates, supplements, and scope of practice.
- `domains/sleep/knowledge-base/` (2 files) — same situation. Sleep science, and the
  recovery/readiness logic that drives autoregulation.
- `domains/training/knowledge-base/17-evidence-updates.md` and
  `18-special-populations.md`
- `domains/ai-coach/adherence-and-behavior-change.md`
- `domains/platform/base44-build-playbook.md`

**Findings that change existing product assumptions (not just additive):**

1. **Nutrition prescription may be outside the owner's scope of practice.** The owner
   is a CPT, not an RD. Generating individualized calorie/macro targets resembles
   prescribing an individualized meal plan, which is restricted in some US states and
   is a liability exposure regardless of licensure regime. **Three postures are laid
   out in `nutrition/knowledge-base/03-safety-and-scope.md` §1; none is chosen.** This
   needs legal review, not an engineering default. It is genuinely viable to ship v0
   with training + tracking and no generated nutrition targets.

2. **HRV is substantially less reliable than fitness-tech marketing implies** — it does
   not reliably predict overreaching (the exact use case it's marketed for), RMSSD has
   many confounders, paradoxical rises under stress are documented, and it saturates in
   trained athletes. The autoregulation loop should weight subjective readiness and RPE
   drift above HRV. Wearables are an enhancement, not a prerequisite.

3. **Cycle-syncing is not evidence-supported.** Do not build cycle-phase-based program
   generation — recent reviews find no effect of cycle phase on strength performance or
   adaptation. Building it would contradict the product's core differentiator.

4. **The library's "2x/week frequency beats 1x at equal volume" claim is overstated for
   hypertrophy** per the 2025 dose-response meta-regression (frequency effect is
   negligible for hypertrophy, positive for strength). Corrected in `17-evidence-updates.md`
   rather than edited in place in `02-bodybuilding.md`, so the original library stays intact.

5. **Base44's documented weakness is complex conditional business logic and edge
   cases** — which is precisely the shape of periodized program generation, entitlement
   gating, and safety refusals. The playbook's central recommendation is to split the
   build: let Base44 generate the ordinary CRUD 80%, and keep the hard 20% (coaching
   logic, safety rules) as explicit, testable, deterministic logic rather than
   improvised generation.

**Recommendation logged, not decided:** treat Base44 as a genuine build target but keep
the coaching logic and knowledge base in this repo as source of truth, so the hardest-
to-rebuild asset never lives only inside the platform (playbook §5, option b→c).

**Verification gap:** `docs.base44.com` was not reachable from the research environment
(network egress policy). Base44 platform specifics in the playbook are drawn from
public marketing content and third-party reviews, and still need confirmation against
current docs — same standing caution as `base44-architecture.md`.

**New open questions raised:** scope-of-practice posture (blocking for nutrition
features); whether v0 ships nutrition prescription at all; which wearable integrations,
if any; how readiness is presented (composite score vs. components); what the coach
does when it detects disengagement; whether adherence data feeds the owner's outreach
funnel (tone risk); supplement recommendations vs. future affiliate revenue (conflict
of interest).

---

### 2026-08-10 — Community deferral REVERSED; fix 01 specified

**Decision 10: community is back in scope.** Supersedes the 2026-08-10
deferral decision. The owner asked to proceed with community features, get
them hooked up, and make sure they operate properly.
Reasoning: owner's explicit call, reversing their own earlier deferral.
Updated: `domains/community/README.md` (Status: ACTIVE), `agents/CLAUDE.md`,
`vision/product-vision.md`, `domains/data/data-model.md`.

**Critical context the reversal surfaced — what's built is not what was
specced.** The live app assessment found community features already exist,
but they are a *different product* from the spec:
- **Built:** consumer social network — `SocialPost`, `Friendship`,
  `PostLike`, `social-feed`/`social-friends`/`social-leaderboard`
  functions, `Community.jsx`. **Open to all users**; `SocialPost` supports
  `visibility: public`.
- **Specced:** coaching community — `CourseContent`, `LiveSession`,
  `LiveSessionAttendee`, `ForumThread`/`ForumPost`. **Gated to active
  `CoachClientRelationship` only.** None of it built.

**This makes the access model contested rather than decided.** Three
options are laid out in `domains/community/README.md`; recommendation is
option A (run the open social layer and a gated coaching community as two
distinct things, since gating the social layer would strip retention value
from exactly the free-tier users the funnel depends on). **Not decided —
needs the owner's call.**

**Newly urgent, not theoretical:** moderation. Public user-generated
content is already possible in the built app, with no report path, no
admin remove capability, and no community guidelines observed. That is a
prerequisite before real users, not a nicety.

**Fix 01 (entitlement bypass) specified but NOT applied.** Full patch,
rationale, and a six-case verification plan written to
`logs/fixes/01-entitlement-bypass.md`. It adds a server-side entitlement
gate to `generate-program` using only existing schema fields (fails closed
on `is_premium`, admin/Pro/owner bypass), plus secondary RLS hardening
options for `WorkoutProgram`. **Blocked on Base44 MCP re-authentication** —
the OAuth flow cannot be completed from a non-interactive cloud session.

**Nutrition scope of practice:** explained to the owner in plain terms; they
acknowledged and waved it off as non-blocking. Recorded as such — it is
**not** resolved, and remains a live exposure since the app already ships
calorie/macro targets with no health screening. Revisit before taking money
from real users.

Still open: community access model (contested); moderation design; native
vs. web; multi-tenant coaching (not planned); SKU build sequencing; the
nutrition scope-of-practice posture; Base44 GitHub sync/export/plan-gating
verification.
