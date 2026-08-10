# PART 0 — THE NUTRITION FRAMEWORK

**Companion to `domains/training/knowledge-base/00-framework.md`.** Same role: learn
this once and every nutrition prescription in the product becomes readable. The AI
coach should reason from this file, not from generic model knowledge about dieting.

> **Read the scope-of-practice limits in `04-safety-and-scope.md` before building
> anything that outputs nutrition advice to a user.** Some of what follows is
> outside what a certified personal trainer may legally prescribe in some
> jurisdictions. That is a product-design constraint, not a footnote.

---

## The hierarchy of what actually matters

Nutrition advice fails when it inverts this order. Almost everyone obsesses over
tier 4 while ignoring tier 1.

| Tier | Lever | Why it ranks here |
|---|---|---|
| **1** | **Energy balance** | Determines whether bodyweight goes up, down, or holds. Nothing below overrides it. |
| **2** | **Protein intake** | Determines how much of a gain is muscle vs. fat, and how much of a loss is fat vs. muscle. |
| **3** | **Total carbohydrate & fat** (fuel for the work) | Determines training quality, which determines the stimulus the program actually delivers. |
| **4** | **Meal timing & distribution** | Real but small. Matters most at high training frequency. |
| **5** | **Supplements** | A rounding error for almost everyone. See `03-supplements.md`. |
| **6** | **Micronutrients & food quality** | Matters enormously for health, barely at all for the acute training outcome. Don't let this ranking imply it's optional. |

**The adherence multiplier:** a tier-1-through-3 plan followed 90% of the time beats a
perfect plan followed 50% of the time. This mirrors the training library's own closing
line — the program is not the variable that determines the outcome.

---

## Energy balance

**Estimating maintenance.** Do not use a formula and treat the output as truth.
Formulas (Mifflin-St Jeor, Harris-Benedict, Katch-McArdle) have individual error bars of
±10–15%, which at 2,500 kcal is ±375 kcal — larger than most deliberate deficits.

**The only reliable method is measurement:**
1. Estimate a starting point from a formula (activity multiplier 1.4–1.9 depending on
   training volume and non-exercise activity).
2. Hold intake steady for 10–14 days.
3. Track bodyweight daily, use the **7-day rolling average** — daily weight is mostly
   water, glycogen, and gut content, and reacting to it is the single most common way
   people wreck an otherwise fine plan.
4. Adjust from the observed trend, not the prediction.

**Adaptive component.** Energy expenditure is not fixed. In a sustained deficit, NEAT
(non-exercise activity thermogenesis — fidgeting, walking, general movement) falls
measurably, and it does so unconsciously. This is why deficits stall even at perfect
adherence, and why "just eat less again" is often the wrong answer — see the diet-break
logic in `01-body-composition.md`.

---

## Protein — the one macro worth being fussy about

**Daily target:** the hypertrophy plateau sits around **1.6 g/kg/day** (≈0.73 g/lb),
with additional benefit up to roughly **2.2 g/kg/day** (≈1.0 g/lb) for some people and
clearly diminishing returns past ~2.0 g/kg. The training library's existing prescription
(0.7–1.0 g/lb on strength/hypertrophy blocks) sits correctly inside this band and does
not need revising.

**Raise the target, toward the top of the band or slightly beyond, when:**
- In an energy deficit (protein needs rise as calories fall — this is when muscle is
  actually at risk)
- Older (anabolic resistance is real; the per-meal threshold rises with age)
- Very lean and getting leaner
- Very high training volume

**Per-meal distribution.** Roughly **0.3 g/kg per meal** (≈20–40 g for most people)
maximizes the muscle protein synthesis response per feeding. Even distribution across
3–5 meals produced ~25% higher 24-hour muscle protein synthesis than a skewed pattern
(most protein at dinner) in controlled work. The practical rule: **protein at every
meal, not most of it at one.**

**Leucine threshold.** Each feeding needs roughly **2.5–3 g of leucine** to trigger
mTORC1 and get a full MPS response. Animal proteins and whey hit this at ~20–25 g of
protein; most plant proteins need a larger dose or blending to get there. This is the
mechanism behind the per-meal number above, and the reason "total daily protein" alone
under-describes the problem for plant-based users.

---

## Carbohydrate and fat — fueling the actual work

Once protein and total calories are set, the carb/fat split is largely a **performance
and preference** decision rather than a body-composition one.

**Carbohydrate scales with training demand, not with a fixed ratio:**

| Training load | Carbohydrate |
|---|---|
| Low / rest day / low-volume lifting | 3–5 g/kg |
| Moderate (most lifters, ~1 h/day) | 5–7 g/kg |
| High endurance (1–3 h/day) | 6–10 g/kg |
| Very high (>3 h/day, stage races, selection prep) | 8–12 g/kg |

**Fat floor:** do not drop below **~0.5 g/kg/day** (many sources say 20% of calories)
for any sustained period — hormonal and micronutrient-absorption consequences follow.
This floor matters most in aggressive cuts, which is exactly when people violate it.

**Intra-session fueling** (already referenced in the training library, restated here as
the general rule):
- Sessions under ~75 min: water is generally sufficient.
- 75 min–2.5 h: **30–60 g carbs/hr**.
- Over 2.5 h: up to **90 g carbs/hr**, requiring mixed glucose:fructose sources to
  exceed the ~60 g/hr single-transporter ceiling. **Gut tolerance for this is trained,
  not innate** — practice it in training or it will fail on event day.

---

## Hydration

- **Baseline:** ~30–40 ml/kg/day, plus replacement of sweat losses.
- **Sweat rate** is individual and measurable: weigh naked pre- and post-session,
  add fluid consumed. Each 1 kg lost ≈ 1 L of sweat.
- **Performance decrement** becomes meaningful around **2% bodyweight loss** in fluid.
- **Sodium** matters in heavy/hot/long sessions — plain water alone in an
  endurance context with high sweat loss risks hyponatremia, which is more dangerous
  than mild dehydration. Do not tell users to "drink as much as possible."

---

## Timing — real, small, and usually oversold

- **The "anabolic window" is wide,** not 30 minutes. Total daily protein and calories
  dominate. Peri-workout timing matters more as training frequency rises and as the
  gap between sessions shrinks.
- **Pre-session:** meaningful carbohydrate 1–3 h prior improves quality of hard
  sessions. Trivial for easy sessions.
- **Post-session:** matters most when the next session is <8 h away (two-a-days,
  hybrid training's doubles). Otherwise "eat a normal meal within a few hours" is
  the honest prescription.
- **Pre-sleep protein** (~30–40 g casein/slow-digesting) has modest support for
  overnight MPS. A genuine tier-4 lever — worth mentioning, not worth stressing about.

---

## How this connects to the training library

Nutrition targets must derive from the **active program phase**, not be set
independently. The training knowledge base already encodes the phase logic:

| Training block | Nutrition posture |
|---|---|
| Accumulate (Wks 1–4) | Maintenance to slight surplus; fuel the volume ramp |
| Intensify (Wks 5–8) | Maintenance or surplus; carbs support intensity |
| Realize (Wks 9–11) | Fuel performance; **not the block to run a deficit** |
| Taper/Test (Wk 12) | Maintain, hydrate, don't experiment |
| Bodybuilding surplus | +200–350 kcal, gaining 0.25–0.5% BW/wk (already in `02-bodybuilding.md`) |
| Fight camp / weight cut | See the weight-cut caution in `13-combat-conditioning.md` — this is the highest-risk nutrition context in the library |

**The interference rule for nutrition:** you cannot run a meaningful deficit and a
peaking block simultaneously and get both. The training library already says this in
several places; the coach must not prescribe around it.

---

## Sources

- [ISSN position stand: protein and exercise](https://pmc.ncbi.nlm.nih.gov/articles/PMC5867436/) — protein supplementation meta-analysis
- [Protein and muscle growth: what the evidence shows](https://foodmedcenter.org/the-science-of-protein-and-muscle-growth-what-the-evidence-really-shows/)
- [Leucine threshold and per-meal dosing](https://drdidwal.com/leucine-threshold-explained-how-much-protein-per-meal-to-build-muscle-science-based-guide)
- [Protein timing and hypertrophy](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3529694/)
