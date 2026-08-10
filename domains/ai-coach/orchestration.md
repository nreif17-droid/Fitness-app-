# AI Coach Orchestration

## The job in one sentence

Take a person's stated goals, current data, and history, and decide what
they should do next — the same judgment call a good human coach makes,
grounded in the actual training-science knowledge base rather than
generic LLM knowledge.

## Why the knowledge base matters

An LLM prompted cold to "build me a running program" will produce something
plausible-sounding but not necessarily sound — it won't reliably apply
things like the 10% weekly mileage rule, 80/20 polarization, or proper
deload timing unless those principles are actually in context. The
`domains/training/knowledge-base/` files exist so the coach is reasoning
from a specific, internally consistent methodology (see `00-framework.md`
for the shared skeleton every modality file follows) instead of improvising
sports science per request. Any prompt sent to the model for program
generation or adaptation should include the relevant knowledge base file(s)
as grounding context, not rely on the model's general training.

## Three distinct AI jobs — don't conflate them

1. **Intake / goal clarification** — conversational, figures out what the
   person actually wants and what's true about their current state and
   constraints. Output: a filled-out AthleteProfile and one or more Goals.
2. **Program selection & generation** — given a profile and goals, choose a
   ProgramTemplate (or combination — see the legal-pairings table in
   `16-integration-guide.md`) and instantiate it as sessions. This can mostly
   be templated logic (goal → modality → level) rather than a heavy AI call,
   at least in early phases — resist reaching for a large model where a
   decision tree does the job more cheaply and more predictably.
3. **Adaptation** — the ongoing loop reading SessionLog/NutritionLog/
   SleepLog/ReadinessCheckIn and deciding whether the plan holds, needs an
   early deload, needs a load adjustment, or needs to flag a red-flag
   pattern to the user (the stop-list in `00-framework.md` and each
   modality's own cautions). This is the part that most needs to be honest,
   not encouraging — a coach that never tells someone to back off isn't
   a coach.

## Guardrails worth deciding early, not late

- **The coach should not silently override red-flag signals.** If logged
  data matches a stop-list pattern (joint pain that worsens through warm-up,
  RHR elevated 10+ bpm for 3+ days, the overreaching triad), the product
  should surface it to the user directly and suggest professional
  evaluation — not just quietly adjust the program.
- **The coach is not a medical or nutrition-licensing substitute.** Anything
  that starts to look like individualized medical or clinical nutrition
  advice (rather than general training-load guidance) needs a clear
  boundary — worth deciding what that boundary is before it comes up in
  production rather than after.
- **Adaptation decisions should be logged and visible** (`CoachDecisionLog`)
  — the person should always be able to see why their program changed, not
  just that it changed. This is also what makes the system debuggable.

## Not yet designed

- The actual prompting/RAG strategy for pulling the right knowledge base
  slice into context per decision (a full-text file per modality is fine for
  a v0; retrieval will need to get more precise as the knowledge base grows
  past what fits in a single context window — e.g. once combat conditioning,
  Olympic lifting, and the rest are joined by nutrition and sleep science
  content of similar depth).
- Where the model calls actually happen — inside Base44's built-in AI
  agent/superagent features, or via an external call this repo's backend
  code makes. This is a platform-capability question to resolve against
  current Base44 docs, not assume.
- ~~The "collective consciousness" cross-user learning layer referenced in
  the vision doc~~ — **decided 2026-08-10: per-user memory only, no
  population-level layer.** Out of scope permanently unless revisited, not
  just deferred (see `logs/decisions.md`).
- **Corrected 2026-08-10** (see `logs/decisions.md`): the three jobs above
  are written correctly for the *primary* case — the AI talks directly to
  the end user (self-serve, no human coach in the loop) for the large
  majority of users. What's not yet designed is the **admin-coached path**:
  for users with a `CoachClientRelationship` to the owner, does the admin
  see/approve AI output before the client does, does the client still get
  the same AI-direct experience with the admin able to intervene
  asynchronously, or does it vary by job (e.g. AI-direct for daily
  adaptation, admin-reviewed for the initial program and re-goal points)?
  Small in scope — it's a visibility/approval variant on the existing three
  jobs, not a new job — but not designed yet.
