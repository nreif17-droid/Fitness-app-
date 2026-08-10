# Base44 Build Playbook

**Purpose:** how to actually get quality output from Base44 for *this* app, and an
honest assessment of where the platform's limits sit relative to what this product
needs. Written 2026-08-10 in response to a direct concern about whether Base44 can
deliver something genuinely valuable and reliable.

> **Verification note:** `docs.base44.com` was not directly reachable from the
> environment this research was done in (blocked by network egress policy). Everything
> below is drawn from Base44's public marketing/blog content, third-party reviews and
> tutorials, and general AI-app-builder production research. **Treat platform
> specifics as needing confirmation against the current docs before you depend on
> them** — this is the same standing caution already in `base44-architecture.md`.

---

## 1. The honest capability assessment

### What Base44 is genuinely good at

- **Speed to a working thing.** Full-stack generation, managed DB/auth/storage/hosting,
  no DevOps. Strong first-pass output.
- **CRUD and forms.** Logging screens, dashboards, list/detail views, basic charts.
- **Standard integrations.** Stripe, email, Slack, Sheets, Notion, Airtable and 20+
  others are advertised connectors.
- **Iteration loop.** Fast edit-preview cycles; easy to try things.
- **AI agents in-app** — agents that understand your app's data, can call backend
  functions, search the web, and follow guidelines you define. **You can choose the
  model, including Anthropic Claude**, and shape persona/tools. This matters a lot for
  this product (see §2).
- **Backend functions** — create/read/update/delete entities, trigger notifications and
  automations. **Gated to the $40 Builder plan** — meaning the interesting version of
  this app is not on the free tier.

### Where it struggles — and why it matters here

Third-party reviews converge on the same limitations, and they land uncomfortably close
to this product's hard parts:

- **Complex business logic.** Multi-step state machines, intricate conditional logic,
  and edge cases are where it "generates something that looks right but fails on edge
  cases you didn't anticipate." **A 12-week periodized program with autoregulated
  deloads, entitlement-gated week visibility, and safety refusal paths is exactly this
  shape.**
- **Consistency degrades with complexity.** Quality "can drift on more complex work";
  behavior gets less predictable on larger projects.
- **Vendor lock-in is real.** Data lives in Base44's managed Postgres; migrating off is
  "genuinely hard," with no clean export-everything button, and **GitHub code export
  was still in beta on most plans as of mid-2026.** This directly affects the
  repo-as-source-of-truth workflow this project assumes — verify current state before
  committing to it.
- **Consensus use case:** excellent for MVPs, prototypes, internal tools, and idea
  validation; weaker for long-term apps with real users and complex workflows.

### The production-readiness gap (not Base44-specific — all AI builders)

Research on AI-generated apps in production identifies a consistent failure pattern:
**auth, data integrity, error handling, and authorization are where things break** —
because AI code generation optimizes for the happy path. Invalid input, expired tokens,
concurrent session conflicts, brute-force patterns, and authorization edge cases are
typically not in the prompt and therefore not in the output. The risk isn't "bad AI
code," it's **code deployed without meaningful security validation**, where incomplete
logic silently weakens authentication or data protection.

The stated boundary is direct: vibe-coded apps are production-ready for internal tools,
low-stakes apps, and MVPs with guardrails — **but not for customer-facing apps handling
payments or PII without E2E test coverage, CI, monitoring, and a security review of auth
and payment flows.**

**This app is customer-facing, handles payments (tier unlocks), and stores health data —
which is more sensitive than ordinary PII.** That doesn't mean don't use Base44. It
means the security/validation work is not optional, and it won't happen by default.

---

## 2. The architectural insight that should drive everything

**Split the product into two categories and treat them completely differently.**

| | **The ordinary 80%** | **The hard 20%** |
|---|---|---|
| **What** | Auth, profile, logging screens (nutrition/sleep/weight/sessions), dashboards, charts, entitlement checks, Stripe purchase flow, admin views, messaging | Program generation from the knowledge base, adaptation logic, red-flag detection, safety refusals, autoregulation decisions |
| **Base44 fit** | **Strong.** This is exactly what it's for | **Weak.** Complex conditional logic + correctness requirements + safety consequences |
| **Approach** | Let Base44 generate it. Iterate in-platform. | **Do not let Base44 improvise this.** Implement as explicit, testable logic — deterministic rules where possible, and a tightly-specified LLM call (Claude, via Base44's model selection or a backend function) where genuine reasoning is needed, always grounded in the knowledge-base files |

**Why this split is the whole answer to the quality concern:** the parts of this app
where "looks right but fails on edge cases" is merely annoying are the parts Base44 is
good at. The parts where that failure mode is *dangerous* — prescribing load
progression, missing a red flag, under-fueling a user — are the parts you keep on a
short leash. The product's differentiator (a coach grounded in real methodology) is
also its highest-risk surface. Those should not be generated by a vague prompt.

**Concretely:**
- The 12-week block structure, deload weeks, tier-gated week visibility, and the
  progression rules are **deterministic**. They're already fully specified in
  `domains/training/knowledge-base/00-framework.md`. Write them as explicit logic, not
  as "AI, figure out the program."
- The autoregulation decision table in
  `domains/sleep/knowledge-base/01-recovery-and-readiness.md` is **deterministic**.
  Same treatment.
- The red-flag stop list and nutrition refusal categories are **hard rules that must
  never be probabilistic.** An LLM deciding whether to surface a possible stress
  fracture is the wrong architecture.
- Genuine LLM reasoning belongs in: intake conversation, goal clarification, explaining
  *why* a change happened in natural language, and composing session-level detail
  within already-validated constraints.

---

## 3. Prompting playbook

### Core principles (from Base44's own guidance and third-party guides)

1. **Be specific and descriptive.** Spell out core features, user interactions,
   business logic (rules, workflows, calculations), and desired outcomes. More detail in
   the prompt produces better first-pass output and **reduces revision cycles and total
   credit spend.**
2. **Structure beats length.** A well-structured five-sentence prompt outperforms a
   vague paragraph regardless of word count. Don't dump; organize.
3. **Explain your reasoning — the *why* is often more useful than the *what*.** When
   Base44 knows the intent, it makes better choices about details you didn't specify.
   For this app: "this is a deload week, so volume must drop ~40% while intensity holds"
   tells it far more than "make week 4 easier."
4. **Use negative constraints.** Say what you don't want ("no pop-ups," "don't add a
   social feed," "do not auto-generate nutrition targets in this screen").
5. **Use Discuss Mode for planning** — brainstorm and explore approaches **without
   affecting the live app or consuming credits.** Use this before every significant
   change, not after something breaks.
6. **Start with data, not AI.** The most-repeated mistake is focusing on the AI too
   early and the data model too late.

### The sequence for this app specifically

**Phase 0 — Schema first, in Discuss Mode.**
`domains/data/data-model.md` is already written and is the spine. Feed it in
deliberately and get the schema right before a single screen exists. Expect to iterate
the schema based on what real screens reveal — that's normal, and it's much cheaper now
than after data exists.

**Phase 1 — The boring, high-value core.** Auth, profile/onboarding, and the logging
loop (session, nutrition, sleep, weight, readiness check-in). This is what makes the
free tier genuinely useful, it's what generates the data the coach needs, and it's
squarely inside Base44's strengths. **Ship this and use it yourself before building the
coach.**

**Phase 2 — Program instantiation, deterministic.** ProgramTemplate + ProgramInstance +
Session generation from a fixed template, with tier-gated week visibility. Still no AI
required — this is table-driven logic off the knowledge base.

**Phase 3 — The coach.** Adaptation logic (deterministic rules first), then the LLM
layer for conversation and explanation. Grounded in knowledge-base files, per the
existing orchestration doc's standing rule.

**Phase 4 — Payments, then admin/coaching layer.**

**Deferred — community** (already an explicit decision; see `domains/community/`).

### Prompt anatomy that works for this project

```
CONTEXT: What this app is, who the user is, what phase we're in.
GOAL: The single outcome this change should produce.
DATA: Which entities are involved and how they relate.
RULES: The specific business logic, stated as rules — including edge cases.
CONSTRAINTS: What NOT to do. What not to touch.
WHY: The reasoning, so it can fill gaps sensibly.
```

**Feed the knowledge base explicitly.** When asking for anything program- or
coaching-related, paste or reference the relevant `knowledge-base/` file. The standing
rule from `orchestration.md` applies to *your prompts to Base44*, not only to the app's
runtime prompts: if you're asking it to build program logic and the relevant knowledge
base file isn't in context, you're asking it to improvise sports science.

### Iterating without breaking things

- **One change per prompt.** Large multi-part requests are where consistency degrades.
- **Discuss Mode before edit mode** for anything structural.
- **Verify after each change** rather than batching — the failure mode is silent, so
  the only defense is looking.
- **Keep this repo as the spec of record.** When Base44 and the docs disagree, the docs
  are what you meant; update whichever is actually wrong, deliberately.

---

## 4. Production-readiness checklist

Do not skip these because the app "works." These are the documented gaps in
AI-generated applications, and this app's data is health data.

**Auth & authorization**
- [ ] Can user A read user B's logs, programs, or messages? Test this explicitly.
- [ ] Is the admin role actually enforced server-side, or only hidden in the UI?
- [ ] Does `CoachClientRelationship` correctly gate admin access — and correctly
      *revoke* it when status becomes `ended`?
- [ ] Session expiry, token handling, password reset flow.

**Entitlements (this is money and it's exactly the "conditional pricing logic" reviews
warn about)**
- [ ] Can a free user reach Week 5+ content by manipulating a request?
- [ ] Is week-visibility gating enforced server-side, not just in the UI?
- [ ] Does a lapsed Pro subscription correctly fall back to separately-owned
      per-modality entitlements without revoking them?
- [ ] Invite code: can it be redeemed more than `max_redemptions`? Race conditions on
      simultaneous redemption?

**Data integrity**
- [ ] Input validation on every logged number (a 5,000 kg squat, a negative sleep
      duration, a date in 2099).
- [ ] What happens on concurrent edits to the same ProgramInstance?
- [ ] Backups. Restore actually tested, not assumed.

**Safety logic**
- [ ] Red-flag detection fires reliably — test with synthetic data that should trigger it.
- [ ] Nutrition floors and rate caps cannot be bypassed.
- [ ] Refusal categories actually refuse.

**Privacy/compliance**
- [ ] Health data handling and consent. **Health data raises the stakes above ordinary
      PII** — check what obligations apply in your jurisdictions.
- [ ] `marketing_opt_in` genuinely gates outreach; unsubscribe works.
- [ ] Data export and deletion paths exist (users will ask, and some regimes require it).

**Operational**
- [ ] Error monitoring — do you find out when something breaks, or does the user?
- [ ] Rollback path.

---

## 5. The strategic question worth answering early

Given lock-in is real and export was in beta, decide deliberately which of these you're
doing:

**(a) Base44 as the product.** Accept the platform, stay inside its strengths, keep the
hard 20% tightly specified. Fastest path to a real product; accepts the ceiling and the
lock-in.

**(b) Base44 as the validation instrument.** Use it to prove people want this and to
learn what the product actually is, with an explicit expectation of rebuilding on
custom infrastructure if it works. This is the pattern the review literature
recommends — iterate and validate, then transition to custom engineering.

**(c) Hybrid.** Base44 for the app shell and CRUD; the coaching brain as an external
service the app calls, so the valuable, differentiating logic isn't trapped in the
platform.

**Recommendation: (b) with an eye toward (c).** Build it in Base44 — genuinely, not as
a throwaway — but keep the coaching logic and the knowledge base in *this repo* as the
source of truth, so the thing that's actually hard to rebuild never lives only inside
the platform. That is already this repo's stated purpose, which means the current
architecture is pointed the right way; the discipline is just not to let it drift.

**Not a decision yet** — logged as a recommendation. See `logs/decisions.md`.

---

## Sources

- [Base44: best prompts for app building](https://base44.com/blog/best-prompts-for-app-building)
- [Base44: 10 techniques for building professional apps](https://base44.com/blog/base44-ai-app-builder)
- [Base44 support docs: writing effective prompts](https://docs.base44.com/Getting-Started/Prompt-guide) *(not directly reachable from this environment — verify yourself)*
- [Base44 docs: setting up an AI agent for your app](https://docs.base44.com/Building-your-app/AI-agents-for-apps)
- [LOW/CODE: Base44 prompting guide](https://www.lowcode.agency/blog/base44-prompting-guide)
- [LOW/CODE: build an AI app with Base44](https://www.lowcode.agency/blog/build-ai-app-with-base44)
- [No Code MBA: Base44 ultimate guide](https://www.nocode.mba/articles/base44-ultimate-guide)
- [Edana: Base44 from prototype to full production](https://edana.ch/en/2026/03/16/how-to-build-an-application-with-base44-a-comprehensive-guide-from-prototype-to-full-production/)
- [Base44 review: what works and what doesn't in 2026](https://www.zite.com/blog/base44-review)
- [Base44 review: the runtime you don't own](https://future-stack-reviews.com/base44-review/)
- [Base44 review after 8+ real apps](https://justinmckelvey.com/blog/base44-review)
- [Is vibe coding good enough for production?](https://www.mindstudio.ai/blog/is-vibe-coding-good-enough-for-production-apps)
- [Vibe coding security checklist 2026](https://www.nxcode.io/resources/news/vibe-coding-security-checklist-ai-built-apps-2026)
- [Securing AI-generated applications at scale (Invicti)](https://www.invicti.com/blog/web-security/vibe-coded-app-security-how-to-secure-ai-generated-applications-at-scale)
- [Production readiness checklist for vibe-coded apps](https://blog.vibecoder.me/production-readiness-checklist-vibe-coded-apps)
