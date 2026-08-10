# Build Log

Session-by-session record. Newest entry on top.

---

### 2026-08-10 — Wedge confirmed; community deferred; opt-in UX confirmed
Owner confirmed the Hybrid Training wedge recommendation — now decided,
promoted into its own "Launch wedge" section in `vision/product-vision.md`.
Community is explicitly deferred (not cut): spec stays documented but
nothing gets built, prompted to Base44, or added to a live schema until
the core AI-coach loop is proven — marked "Status: DEFERRED" across
`domains/community/README.md`, `data-model.md`, and added as a standing
reminder in `agents/CLAUDE.md` (also cleaned up a stale line there that
still called monetization undecided). Owner also open to running community
on a separate platform/tool entirely (Zoom link, separate app) rather than
building it in-app when it's revisited. Email opt-in UX confirmed as a
plain toggle, off by default, in `data-model.md`.

**Next:** connect to Base44 and verify sync/export/plan-gating; decide SKU
build sequencing and native vs. web when it's time to build; revisit
community once the core loop is live and proven.

---

### 2026-08-10 — Comped access persistence + community gating resolved
Two more open questions closed. Comped app access (from a `CoachInviteCode`
redemption) is permanent even after the coaching relationship ends — matches
how purchased Tier 1/2 unlocks already work. What's tied to the
relationship specifically (`CoachMessage`, community access) ends with it.
This also fully resolves community access gating: it's exclusively an
active-coaching-client benefit, no separate purchasable membership in v0.
Updated `data-model.md`, `domains/community/README.md`,
`vision/product-vision.md`. Owner also reconfirmed no Shaolin/combat-sports
background — doesn't change the Hybrid Training wedge recommendation,
still awaiting explicit confirmation.

**Next:** get the wedge recommendation confirmed or redirected; verify
Base44's video/live-call capability; connect to Base44.

---

### 2026-08-10 — Background clarified, lead capture built, community proposed
Owner's real background: certified personal trainer + genuine hybrid
athlete, not combat sports (that's an interest, not expertise). Recommended
Hybrid Training as the launch wedge on that basis — proposed in
`vision/product-vision.md`, awaiting confirmation, not logged as decided.
Built lead capture (`User.marketing_opt_in`) for the owner's real coaching
funnel. Sketched a new Community domain (coursework/video, live calls,
forum) in a new `domains/community/README.md` plus lightweight entities in
`data-model.md` — flagged two real open questions (access gating, Base44
video/live-call capability) rather than guessing. Added an "Adjacent but
not an AI job" note to `orchestration.md` to keep outreach/community
scoped as human-run, separate from the AI coach's three jobs.

**Next:** get the wedge recommendation confirmed or redirected; decide
community access gating; verify Base44's video/live-call capability;
connect to Base44.

---

### 2026-08-10 — Tier expiry resolved; wedge still open, audience context gathered
Tier 1 ($18) / Tier 2 ($45) per-modality unlocks are permanent, no expiry —
buy once, own it forever on that account. Pro ($200/yr) is the only tier
that expires, annually. Updated `data-model.md` and `vision/product-vision.md`.
On the wedge: owner has ~340 low-interaction Instagram followers and zero
clients currently, so there's no existing audience to leverage in any
particular modality — noted in the vision doc as context, not yet a
decision. Still need to ask about the owner's own personal training
background directly (the knowledge base's "Shaolin discipleship track"
reference is an open thread, not confirmed).

**Next:** land the wedge; decide SKU build sequencing; decide whether
comped entitlements survive relationship end; connect to Base44.

---

### 2026-08-10 — Personal-client free access via invite code
Added the last piece of the monetization picture: the owner's own
(offline-paid) coaching clients get the whole app free, no gates, via a
redeemable `CoachInviteCode` at onboarding that both creates the
`CoachClientRelationship` and grants a comped Pro-tier `Entitlement` in one
step. Updated `data-model.md`, `vision/product-vision.md`,
`orchestration.md`. New open question surfaced: does the comp survive the
coaching relationship ending. Reasoning in `logs/decisions.md`.

**Next:** land the launch-wedge-modality conversation; decide tier expiry
semantics, SKU build sequencing, and whether comp survives relationship end;
connect to Base44.

---

### 2026-08-10 — Monetization tiers + admin-oversight boundary decided
Two more decisions resolved with the user. Monetization: tiered per-modality
unlocks (Free = Wks 1–4, $18 Tier 1 = Wks 1–8, $45 Tier 2 = full 12 wks +
ongoing, $200/yr Pro = Tier 2 everywhere) — maps directly onto the existing
Accumulate/Intensify/Realize block boundaries in the knowledge base, so it's
a paywall on content that already exists. Admin oversight: no approval
gate — AI acts the same for self-serve and coached users, admin gets
after-the-fact visibility/override plus a new direct-messaging channel to
coached clients. Added `Entitlement` and `CoachMessage` to
`domains/data/data-model.md`, updated `vision/product-vision.md` and
`domains/ai-coach/orchestration.md` to match. Full reasoning in
`logs/decisions.md`.

**Next:** land the launch-wedge-modality conversation (in progress —
exploring the owner's own background/audience as an input); decide tier
expiry semantics and SKU build sequencing; connect to Base44.

---

### 2026-08-10 — Corrected target-user decision: B2C self-serve + owner's coaching layer
The "coach-facing tool" decision from earlier this session was wrong — user
corrected it directly. Actual shape: primary product is public, self-serve
B2C (anyone signs up, AI coaches them with no human in the loop); the owner
additionally runs their own coaching clients through the same system as
admin, with visibility/override on that specific relationship. Not a
multi-coach marketplace. Updated `vision/product-vision.md`,
`domains/data/data-model.md` (added `User.role` and
`CoachClientRelationship`), `domains/ai-coach/orchestration.md`
(admin-coached path flagged as not-yet-designed, small in scope), and
`agents/CLAUDE.md`. Full reasoning in `logs/decisions.md`.

**Next:** design the admin-coached path in orchestration (does the admin
review/approve, or just get visibility with override); connect to Base44;
resolve remaining open questions (launch wedge modality, monetization,
native vs. web).

---

### 2026-08-10 — Landed scaffold to GitHub + first two decisions resolved
Pushed the scaffold as the initial commit to `nreif17-droid/Fitness-app-`
(branch `claude/task-clarification-nzd30y`), then a `main` branch and PR.
Resolved two open questions with the user: v0 is a **coach-facing tool**
(not B2C), and AI coach memory is **per-user only** (no cross-user
"collective consciousness" layer). Updated `vision/product-vision.md`,
`domains/data/data-model.md`, `domains/ai-coach/orchestration.md`, and
`agents/CLAUDE.md` to reflect both, and logged reasoning in
`logs/decisions.md`. Surfaced a new follow-on the coach-facing decision
creates: no `Coach`/`CoachClientRelationship` entities exist yet, and
orchestration doesn't yet say who the AI talks to at each step (coach vs.
client) — flagged in place, not designed.

**Next:** connect to Base44; resolve remaining open questions (launch
wedge modality, monetization, native vs. web, Base44 sync/export/
plan-gating verification); design the coach/client data model and
orchestration split.

---

### 2026-08-10 — Scaffold
Set up repo structure (vision/domains/logs/agents), drafted product vision,
data model, AI coach orchestration doc, Base44 platform notes, and split
the 13-modality + framework training library into
`domains/training/knowledge-base/`. Nothing pushed to GitHub or connected
to Base44 yet — that's the next step, manual, outside this environment.

**Next:** decide the open questions in `logs/decisions.md`, then get this
into an actual GitHub repo and connected to Base44.
