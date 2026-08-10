# Build Log

Session-by-session record. Newest entry on top.

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
