# PART 18 — SPECIAL POPULATIONS & INDIVIDUAL VARIATION

Volumes I and II are written for a healthy adult with no significant constraints. Real
users are not uniformly that. This file covers the adjustments the coach must make —
and the cases where it must **not** try.

> **Anything in the refusal categories of
> `domains/nutrition/knowledge-base/03-safety-and-scope.md` overrides this file.**
> Pregnancy, diagnosed conditions, and under-18 users route to a qualified human; they
> are not programming problems to solve.

---

## Female physiology

### The menstrual cycle and training — what the evidence actually says

This is a topic where the popular advice and the evidence have diverged sharply, and
the coach must follow the evidence.

**"Cycle syncing" — structuring training phases around cycle phases — is not supported
by current evidence.** Recent reviews find **no differences in resistance training
outcomes by cycle timing**, and no influence of cycle phase on acute strength
performance or on adaptations to resistance training. This contradicts the widespread
belief that the follicular phase (high estrogen) offers a hypertrophy advantage.

**What *is* true and does matter:**
- **Symptoms vary and are real.** Fatigue, cramping, bloating, and mood changes
  genuinely affect motivation, perceived effort, and session quality for many people —
  even though measured physiological capacity doesn't shift.
- **The correct response is autoregulation, which the library already has.** A user
  reporting low readiness gets the same adjustment regardless of *why* they feel bad.
  No special cycle-based programming layer is needed.
- **Consistency and progressive overload remain the drivers**, same as everyone.

**Product implication:** do not build cycle-phase-based program generation. If cycle
tracking is offered at all, it should be optional, private, consented, and used to
contextualize readiness — not to alter prescriptions on a schedule. Building
"cycle-synced training" would be implementing a popular idea the evidence doesn't
support, in a product whose entire differentiator is being evidence-grounded.

### Where genuine sex differences do warrant adjustment

- **Relative fatigue resistance** — women often tolerate slightly higher volume at a
  given relative intensity and recover faster between sets, particularly on
  lower-body/endurance work. Practical effect: rest periods and set volume can
  sometimes be adjusted, autoregulated rather than prescribed by sex.
- **RED-S / low energy availability risk** is higher in practice, and the consequences
  (menstrual dysfunction, bone density) are more visible. See the nutrition safety file.
  **Loss of menstrual cycle in a training user is a red flag, not a convenient side
  effect** — a widespread and harmful misconception.
- **Bone health and iron status** deserve more attention, especially in endurance
  populations.

---

## Older athletes (roughly 50+)

The training principles don't change. The recovery and tissue timelines do.

- **Anabolic resistance:** a larger per-meal protein dose is needed to trigger the same
  MPS response — push toward the top of the protein band and the higher end of per-meal
  dosing (see nutrition `00-framework.md`).
- **Recovery between hard sessions takes longer.** The most common productive
  adjustment is *frequency of hard days*, not intensity. Older lifters can generally
  still train heavy; they just can't do it as often.
- **Connective tissue and joint tolerance** — longer ramps, more warm-up, more
  attention to exercise selection. The library's mobility module (Part 10) moves from
  optional to strongly recommended.
- **Power declines faster than strength**, which declines faster than endurance.
  Deliberately keeping some fast/explosive work (jumps, throws, short sprints, at
  appropriate volumes) preserves the quality that erodes first and matters most for
  independence later.
- **Deloads matter more, not less.** Consider a 3:1 loading pattern where a younger
  athlete might run 4:1 or 5:1.

---

## Youth and adolescents

**Under-18 users are a refusal category for this product** (see nutrition safety file) —
consent, COPPA-shaped obligations, and physiological considerations all apply. If that
decision is ever revisited, the essentials: resistance training is safe and beneficial
under supervision with technical focus; growth spurts transiently reduce coordination
and raise injury risk; early specialization increases injury and burnout risk; sleep
needs are higher (9–11 h). This is a distinct domain requiring real expertise, not an
adjustment layer.

---

## Beginners vs. returners — a distinction the library should make

The library's beginner tracks assume genuine novices. **Returning trained individuals
are a different population** and are common:

- **Muscle memory is physiologically real** — myonuclei persist after detraining, so
  regaining lost muscle is substantially faster than building it the first time.
- **Cardiovascular fitness returns faster than tissue tolerance.** This is the classic
  returner injury: the engine comes back in weeks, the tendons haven't, and the person
  trains to what their lungs allow. The running module's governing principle applies
  doubly here.
- **Practical rule:** returners can progress *load and intensity* faster than true
  novices, but should respect novice-like timelines on *volume and impact*.
- **Onboarding implication:** `AthleteProfile.training_age_years` shouldn't be a single
  number. "Trained for 5 years, off for 2" is a materially different prescription from
  either "5 years trained" or "untrained."

---

## Individual variation — the honest caveat on everything

- **Responder variance is large.** In controlled training studies, individual responses
  to identical programs range from substantial gains to near-zero. Group means hide this.
- **This is why autoregulation and logged data beat template adherence** — and why this
  product's premise (adapt from actual data) is sound rather than a marketing angle.
- **Practical consequence for the coach's tone:** prescriptions should be framed as
  starting hypotheses to be revised from the user's own data, not as guarantees. The
  library's own expectation-setting (beginners 15–40%, advanced 2–7%) should be quoted
  honestly, including the part where a 5% FTP gain at high training age is a *good*
  outcome.

---

## Sources

- [Current evidence shows no influence of menstrual cycle phase on acute strength performance or adaptations to resistance training (Frontiers)](https://www.frontiersin.org/journals/sports-and-active-living/articles/10.3389/fspor.2023.1054542/full)
- [ACE: Menstrual cycle and resistance training — what the latest research says (2025)](https://www.acefitness.org/continuing-education/certified/april-2025/8846/menstrual-cycle-and-resistance-training-what-the-latest-research-says/)
- [The effect of menstrual cycle phase, symptoms, motivation and readiness on resistance training performance (Sports Medicine)](https://link.springer.com/article/10.1007/s40279-026-02459-8)
- [Power in the flow: how menstrual experiences shape women's strength training performance](https://pmc.ncbi.nlm.nih.gov/articles/PMC11897035/)
