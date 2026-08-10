# PART 1 — BODY COMPOSITION CHANGE

Fat loss, muscle gain, and the honest limits on doing both. This is where most user
goals will actually land, and where most bad advice lives.

---

## Rate is the whole game

The single most common self-inflicted failure is **going too fast.** Both directions.

### Fat loss

| Rate | Deficit | Use when | Lean mass cost |
|---|---|---|---|
| **0.5% BW/wk** | ~250–350 kcal | Already lean, athlete, preserving performance | Minimal |
| **0.5–1% BW/wk** | ~300–500 kcal | **The default recommendation** | Low with adequate protein |
| **1–1.5% BW/wk** | ~700–1000 kcal | Higher body fat, medical urgency, early phase | Rising |
| **>1.5% BW/wk** | Large | Rarely justified outside clinical supervision | High |

**The controlled evidence is unambiguous:** in a trial comparing 0.7%/wk vs. 1.4%/wk
in athletes, the slow group *gained* 2.1% lean body mass and lost 31% of fat mass; the
fast group's lean mass did not change and they lost only 21% of fat mass. Slower was
better on both axes — they lost more fat *and* gained muscle.

**For lean individuals specifically**, the ISSN recommends a maximum **300–500 kcal/day**
deficit targeting **0.5–0.75% BW/wk**. Below ~10% body fat (men) / ~18% (women),
aggressive deficits produce performance collapse and hormonal suppression, not faster
results.

**Expected composition of the loss:** under good conditions (adequate protein,
resistance training, moderate rate), roughly **70–80% fat / 20–30% lean**. Push the
rate up or the protein down and that ratio degrades fast.

### Muscle gain

Realistic rates are far slower than the fitness industry implies, and they decay hard
with training age:

| Training age | Realistic gain |
|---|---|
| Year 1 | 1–1.5% BW/month (novice window — the fastest it will ever be) |
| Year 2–3 | 0.5–1% BW/month |
| Year 4+ | 0.25–0.5% BW/month, or less |
| Advanced/near genetic ceiling | Measured per year, not per month |

**Surplus size:** +200–350 kcal, targeting **0.25–0.5% BW/wk**. Larger surpluses do not
build muscle faster past a point — they add fat, which then costs a cut to remove. The
"bulk hard, cut hard" cycle is mostly a way to spend a year arriving where you started.

### Body recomposition (both at once)

**It is real but conditional.** It works reliably in:
- Untrained beginners
- Detrained people returning (muscle memory — myonuclei persist)
- People with higher body fat
- Anyone with previously inadequate protein intake

**It works poorly or not at all in:** trained, lean individuals near their ceiling. For
those users, the honest answer is to pick a direction and run a block, not to promise
both. **The coach must not promise recomposition to an advanced lean user** — that's
the fitness-app equivalent of overselling, and it destroys trust when the scale and
mirror don't move.

---

## Metabolic adaptation and diet breaks

Sustained deficits produce more than the arithmetic loss of expenditure:
- **NEAT falls** (unconsciously — less fidgeting, less walking, more sitting)
- **Leptin falls**, hunger signaling rises
- **Thyroid output drops** modestly
- **Training output falls**, further reducing expenditure

The result is a stall that isn't a compliance failure. Treating it as one — cutting
calories further — accelerates lean mass loss.

**Diet breaks / refeeds** are the structured answer:
- **Refeed:** 1–2 days at maintenance, carbs elevated, protein held. Weekly-ish.
- **Diet break:** 1–2 weeks at maintenance. Every 6–12 weeks of sustained dieting.
- Both partially restore leptin, NEAT, training quality, and — importantly —
  psychological tolerance.

**Map diet breaks onto the training deload weeks (4, 8, 12).** The program already
pauses there; aligning the nutrition break costs nothing and simplifies the user's
mental model.

---

## Measuring progress honestly

**Bodyweight alone is a poor signal over short windows.** Daily fluctuation of 1–2 kg
from water, glycogen, sodium, gut content, and (for menstruating users) cycle phase
routinely masks real change.

The tracking minimum:
- **7-day rolling average bodyweight**, same conditions (morning, post-void, pre-food)
- **Photos**, 4 angles, same light, same time of day — every 4 weeks
- **Tape measurements** at fixed sites — every 4 weeks
- **Training performance** — the best proxy for whether lean mass is being preserved.
  Strength falling off a cliff in a deficit is the signal to slow down.

**On body fat percentage:** every field method (BIA scales, calipers, most consumer
tools) has error bars wide enough that a single reading is nearly meaningless. Trends
from a consistent method are usable; absolute numbers are not. **The product should not
display a body fat percentage as though it were a precise measurement** — it invites
users to react to noise.

---

## The eating-disorder boundary

This section is a product-safety requirement, not background reading.

Fitness apps that track calories and weight are a documented risk surface for disordered
eating. The features that make the product good (precise tracking, goal-driven targets,
progress graphs) are the same features that harm a vulnerable user.

**Design requirements:**
- **Floor on prescribed intake.** Never generate a target below ~1,200 kcal (women) /
  ~1,500 kcal (men) without explicit clinical oversight — and arguably never at all in
  a self-serve consumer app.
- **Floor on rate.** Refuse to build a plan targeting >1.5% BW/wk loss.
- **Floor on body fat targets.** Do not generate plans targeting sub-essential body
  fat levels.
- **Pattern detection.** Rapid unexplained loss, sustained very low intake, obsessive
  logging patterns, or a stated goal weight in an unhealthy BMI range should trigger a
  supportive, non-clinical message and a referral pathway — not silent compliance.
- **Language.** No "good food"/"bad food" framing, no moralizing about a logged day, no
  streak mechanics that punish a missed log.

See `04-safety-and-scope.md` for the full boundary discussion and
`domains/ai-coach/orchestration.md` for the existing guardrails this extends.

---

## Sources

- [Rate of weight loss and body composition in elite athletes](https://pubmed.ncbi.nlm.nih.gov/21558571/) — the 0.7% vs 1.4%/wk trial
- [Evidence-based bulking and cutting rates](https://www.thebodybuildingdietitians.com/blog/how-fast-should-you-bulk-or-cut-evidence-based-guidelines)
- [Realistic rates of fat loss and muscle gain](https://www.precisionnutrition.com/rates-of-fat-loss-and-muscle-gain)
