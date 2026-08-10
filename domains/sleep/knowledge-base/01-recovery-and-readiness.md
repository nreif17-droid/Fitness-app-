# PART 1 — RECOVERY & READINESS MONITORING

This file governs how `ReadinessCheckIn` data should actually be used by the
adaptation loop. **It is more skeptical than most fitness-tech marketing**, deliberately,
because the failure mode here is a product that confidently reschedules someone's
training on the basis of noise.

---

## The core problem

The coach needs to answer one question daily: *should today's session run as planned,
be reduced, or be replaced?* Every monitoring tool below is an attempt to answer it,
and none of them answer it cleanly on their own.

---

## HRV — useful, oversold, and easy to misuse

**What it is:** variation in the time interval between heartbeats, reflecting the
balance between sympathetic and parasympathetic branches of the autonomic nervous
system. Higher HRV generally indicates parasympathetic (recovered) dominance; lower
indicates sympathetic activation or reduced vagal tone.

**What the evidence supports:**
- **HRV-guided training appears more effective for developing aerobic performance than
  pre-planned training** — this is the strongest claim in its favor, with meta-analytic
  support for VO₂max outcomes.
- HRV has shown value as a **predictor of illness** in elite athletes — arguably its
  most practically useful application.

**What the evidence does not support, and where products routinely overreach:**
- **HRV does not appear to be a reliable predictor of overreaching.** This is the exact
  use case most recovery apps market it for.
- **RMSSD** (the common consumer metric) measures only parasympathetic activity and is
  **susceptible to numerous external confounders** — alcohol, illness, heat, hydration,
  menstrual cycle phase, measurement position, time since waking, even breathing rate
  during measurement.
- **Paradoxical increases under stress are documented** — military, tactical, and
  overreached athletic populations have shown HRV *rising* despite significant
  physiological strain. A single high reading is not proof of recovery.
- **HRV saturation in highly trained athletes** — HRV can fall even as resting heart
  rate improves, making the raw number misleading at the top end.

### Rules for using HRV in this product

1. **Never act on a single reading.** Use a **7-day rolling average** compared against
   the individual's own **28-day baseline** — never against population norms, which are
   meaningless at the individual level.
2. **Trend and direction, not absolute value.**
3. **Require corroboration.** HRV alone should not trigger a program change. It should
   *contribute* to a readiness picture alongside subjective report and resting HR.
4. **Standardize measurement** or don't bother: same time (on waking), same position,
   same duration, before caffeine or phone-scrolling.
5. **Never present HRV as a precise "recovery score" with false confidence.** If the
   product shows a number, it should be honest about what it can and can't tell you.

---

## The full readiness picture — what to actually weigh

Ranked by signal quality per unit of user friction:

| Marker | Weight | Notes |
|---|---|---|
| **Subjective readiness (1–10)**, taken *before* training | **High** | Cheap, fast, and validated. Subjective wellness measures often outperform expensive physiological ones for detecting accumulated fatigue. The training library already prescribes this. |
| **Sleep hours + quality** (last night, and 3-night trend) | **High** | Two consecutive poor nights is already a stop-condition in the training library |
| **Resting heart rate** vs. baseline | **Moderate–high** | Simple, robust, less confounded than HRV. **+7 bpm → swap the hard session** (already in the library); **+10 bpm for 3+ days → stop and reassess** |
| **Session RPE vs. prescribed RPE** | **High** | The best single indicator that exists in this product. If prescribed loads feel 2+ RPE points harder for two sessions running, that's the deload trigger the library already specifies |
| **Muscle soreness / joint discomfort flags** | **Moderate** | Distinguish muscular soreness (normal) from joint/bone pain (red flag) |
| **HRV** | **Moderate, with all caveats above** | Contributory, not decisive |
| **Mood / motivation** | **Moderate** | Part of the overreaching triad; easy to collect, often the earliest signal |
| **Bodyweight trend** | **Moderate** | Rapid unexplained loss ties into RED-S screening (see nutrition `03-safety-and-scope.md`) |

**The design principle:** a readiness system built on one sensor is a novelty. One built
on a short daily self-report plus RPE from the last few sessions is genuinely useful and
requires no hardware at all. **Wearable integration is an enhancement, not a
prerequisite** — worth saying plainly, since it's tempting to gate the good version of
this product behind a device most users won't have.

---

## Autoregulation logic — turning signals into decisions

The training library already defines the decision rules. This file's job is to say what
feeds them:

| Signal state | Action |
|---|---|
| All markers normal | Run the session as prescribed |
| One marker mildly off | Run as prescribed; note it; watch tomorrow |
| Subjective low + RPE drift over 2 sessions | **Take the deload early.** The library is explicit: the calendar serves you, not the other way around |
| 2 consecutive poor sleep nights **or** RHR +7 bpm | Swap the hard session for Z2 or rest |
| RHR +10 bpm for 3+ days | Stop and reassess — possible illness or significant overreach |
| Overreaching triad (appetite loss + sleep disturbance + mood crash **together**) | Stop, reassess, consider ending the block early |
| Any red-flag from the stop list (sharp joint pain, pain worsening through warm-up, numbness/tingling down a limb, chest pain, bone-point tenderness) | **Stop. Surface to the user directly. Suggest professional evaluation.** Never silently adjust around this — existing orchestration guardrail |

---

## Recovery modalities — what's worth recommending

Ranked honestly, because this category is full of expensive interventions with thin
evidence:

**Tier 1 — actually works, free:**
- **Sleep** (see `00-sleep-science.md`) — nothing else is close
- **Adequate energy intake** — under-fueling *is* under-recovering
- **Deload weeks** — programmed, not earned. Already structured into every program
- **Easy aerobic movement** on rest days

**Tier 2 — modest, situational:**
- **Heat (sauna)** — cardiovascular adaptations, possible modest performance benefit
- **Massage / soft tissue work** — good evidence for perceived recovery and soreness,
  weaker for objective performance restoration. Feeling better is not nothing.
- **Compression** — small effects on perceived soreness

**Tier 3 — evidence is mixed, and there's a real catch:**
- **Cold water immersion** — reduces perceived soreness, **but there is reasonable
  evidence it blunts hypertrophy and strength adaptation when used regularly after
  resistance training.** The inflammation it suppresses is part of the adaptive signal.
  Use it to recover *for a competition*, not to recover *from training you want to adapt
  to.* The coach should not recommend post-lifting ice baths during an accumulation
  block.
- **Stretching for recovery** — the mobility module already covers why passive static
  stretching underdelivers

**Tier 4 — largely marketing:**
- Most recovery drinks beyond their protein/carb content, cupping, most percussion-gun
  claims beyond transient soreness relief, "detox" anything

---

## Open questions

- **Wearable integrations** — which, if any, ship at v0 (Apple Health, Garmin, Whoop,
  Oura)? Each is real integration work. The readiness system above is designed to be
  useful *without* any of them, deliberately.
- **How the readiness score is presented to users.** A single composite number is
  appealing and easy to over-trust. Showing components separately is more honest and
  less satisfying. Worth deciding on purpose.

---

## Sources

- [Monitoring training adaptation and recovery using HRV via mobile devices: narrative review](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12787763/) — source for RMSSD limitations, confounders, paradoxical responses, and saturation
- [HRV-based training for improving VO₂max: systematic review with meta-analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7663087/)
- [Training adaptation and HRV in elite endurance athletes](https://pubmed.ncbi.nlm.nih.gov/23852425/)
- [Science for Sport: HRV overview](https://www.scienceforsport.com/heart-rate-variability-hrv/)
