# PART 3 — SAFETY, SCOPE OF PRACTICE, AND THE MEDICAL BOUNDARY

**This file is a product-design constraint, not background reading.** It describes
limits the application must enforce in code and in prompts, not guidance the AI can
weigh against other considerations.

`domains/ai-coach/orchestration.md` already flags "the coach is not a medical or
nutrition-licensing substitute" as a guardrail needing a real decision. This file is
the research behind that decision. **The decision itself is still open** — see the
open questions at the end.

---

## 1. Scope of practice — the legal reality

The platform owner is a **certified personal trainer**, not a registered dietitian.
That distinction has legal force, and it varies by jurisdiction.

**What a CPT can generally do:**
- Provide **general nutrition education** — the role of macronutrients, hydration,
  general healthy-eating principles, public-health-level guidance
- Discuss what a client eats and suggest general improvements
- Share published general guidelines

**What a CPT generally cannot do:**
- Provide **medical nutrition therapy** — reserved for RDs/licensed nutritionists
- **Prescribe individualized meal plans** (in many jurisdictions this is the bright line)
- **Diagnose or treat** conditions (PCOS, IBS, diabetes, thyroid disorders, etc.)
- Recommend supplements to correct a **deficiency** without lab confirmation
- Prescribe therapeutic diets or manage nutrition-related medical conditions

**Jurisdictional variation is significant and real.** In the US, each state regulates
nutrition advice through licensure, statutory certification, or registration —
and the range is wide. Some states restrict specific dietary guidance to registered
dietitians outright; others have no licensure law at all and permit anyone to give
nutrition advice. **A product that ships nationally ships into all of these regimes at
once.**

**Liability does not depend on the licensure regime.** A trainer can be held liable for
negligence or misinformation regardless of whether their state licenses nutrition
practice. Civil action is the exposure, and "the AI generated it" is not a defense the
owner should expect to rely on.

### What this means for the product

This is the uncomfortable part: **an AI coach that generates individualized calorie and
macro targets is doing something that looks a lot like prescribing an individualized
meal plan.** The `NutritionTarget` entity in the data model does exactly this.

Three broad postures, all viable, all requiring a deliberate choice:

| Posture | What it means | Cost |
|---|---|---|
| **A. General education only** | The app explains principles and lets the user set their own targets. No generated individualized prescriptions. | Safest legally; substantially weaker product |
| **B. Generated targets + strong framing** | The app generates targets but frames them explicitly as general educational information, not individualized advice, with clear disclaimers, an onboarding health screen, and hard refusal paths for medical contexts | The likely real answer; needs actual legal review, not a guess |
| **C. RD in the loop** | A registered dietitian reviews or authors nutrition logic | Most defensible; costs money and adds a human bottleneck |

**This needs a lawyer, not an AI's opinion.** Flagged as such in `logs/decisions.md`.
What is written here is research to inform that conversation, and should not be treated
as legal advice or as a decision already made.

---

## 2. Mandatory refusal and referral categories

Regardless of which posture above is chosen, the coach must **not** generate a plan and
must instead route to a qualified human for:

- **Pregnancy and postpartum** — training and nutrition both change materially; this is
  clinical territory
- **Diagnosed eating disorders, or a screening result suggesting one**
- **Diabetes (type 1 or 2)** — carbohydrate prescriptions interact with insulin dosing.
  This is a genuine harm pathway.
- **Cardiovascular disease, uncontrolled hypertension, recent cardiac event**
- **Chronic kidney disease** — protein targets in this document are actively
  contraindicated
- **Anyone under 18** — youth training and nutrition are their own domain, and there
  are consent/COPPA implications on top of the physiological ones
- **Active injury under medical care**, or any red-flag symptom from the training
  library's stop list
- **Any medication with training or nutrition interactions** (beta-blockers invalidate
  heart-rate-zone prescriptions entirely, for one common example)

**Design implication:** the onboarding health screen (`AthleteProfile.injury_history`
already exists; this needs more) should capture these *before* the first program is
generated, and the generation path must be able to refuse. A coach that can't say
"I shouldn't be the one advising you here" is a liability, not a feature.

---

## 3. Low energy availability and RED-S

**The most likely serious harm this app could cause is under-fueling an active user** —
and the design pressure runs directly toward it, because users arrive wanting fat loss
and the app is happy to help them train more and eat less simultaneously.

**Relative Energy Deficiency in Sport (RED-S)** — introduced by the IOC in 2014,
expanding the earlier Female Athlete Triad — describes what happens when energy intake
is insufficient for the sum of training demand and basic physiological function.

**Energy availability** = (dietary energy intake − exercise energy expenditure) ÷
fat-free mass. Below roughly **30 kcal/kg FFM/day**, function starts degrading.

**It affects both sexes.** The older Female Athlete Triad framing caused a decade of
under-recognition in men, and that misconception is still widespread.

**Systems affected:** metabolic rate, menstrual function, bone density and
musculoskeletal health, immunity, protein synthesis, cardiovascular health, endocrine
function, mood, and — the one users notice first — performance.

**Detectable signals the product already collects or could:**
- Weight loss faster than prescribed, especially sustained
- Training performance declining while adherence is high
- Elevated resting HR / suppressed HRV alongside a deficit
- Loss of menstrual cycle (where tracked — this requires deliberate, consented,
  privacy-careful design)
- Sleep disruption + mood decline + appetite change together (the training library's
  existing overreaching triad)
- Recurrent illness or injury, especially bone-stress injuries

**The training library already encodes the most important single rule**, in the fight
camp section: *losing more than 1% of bodyweight per week during a training block means
you're eating a conditioning program's worth of stress into a deficit and getting
neither result.* That rule should be **generalized across every modality**, not left
in the combat sports file.

**Awareness is low even among clinicians** — research reports low RED-S knowledge among
physiotherapists, GPs, gynaecologists, and orthopaedic surgeons, who are typically the
first professionals an affected athlete consults. The product should not assume a user
who "checked with their doctor" has been screened for this.

---

## 4. Required product behaviors

Summarizing the above into what must actually be built:

1. **Health screening at onboarding**, before first program generation, covering the
   refusal categories in §2.
2. **A refusal path** the AI can take — with a supportive, non-alarming message and a
   referral suggestion — rather than always producing a plan.
3. **Hard floors** on generated intake and rate of loss (see `01-body-composition.md`).
4. **Energy-availability sanity check** whenever training volume and intake are both
   known: flag when the combination is implausibly low.
5. **Red-flag escalation that is visible to the user**, per the existing orchestration
   guardrail — the coach surfaces it, doesn't silently adjust around it.
6. **Disclaimers that are actually read** — at onboarding and at the point of first
   nutrition prescription, not buried in a ToS.
7. **Audit trail** — `CoachDecisionLog` should capture nutrition prescriptions and any
   refusals, both for user transparency and for the owner's own protection.

---

## Open questions — decide these deliberately

- **Which scope posture (A/B/C above) does the product take?** Needs legal review.
  Until decided, don't build nutrition prescription as though the answer is B.
- **Does the app ship nutrition prescription at v0 at all**, or does v0 ship
  training + tracking and add nutrition targets once the scope question is settled?
  The core loop works without generated nutrition targets; this is a genuine option.
- **Jurisdiction:** does the app restrict availability by state/country, or ship
  everywhere and rely on framing?
- **Does the owner's CPT liability insurance cover software-delivered advice?**
  Most trainer policies contemplate in-person practice. Worth checking before launch.

---

## Sources

- [ACE: Nutrition scope of practice for personal trainers](https://www.acefitness.org/resources/pros/expert-articles/6248/nutrition-scope-of-practice-what-you-can-do-as-a-personal-trainer/)
- [NSCA: Personal trainers and nutrition advice](https://www.nsca.com/certification/nsca-cpt/essentials-of-personal-training--3rd-edition/excerpts/personal-trainers-and-nutrition-advice/)
- [NFPT: Nutrition and meal planning scope of practice](https://nfpt.com/food-thought-scope-practice-personal-trainer/)
- [RED-S overview (PubMed)](https://pubmed.ncbi.nlm.nih.gov/35915044/)
- [RED-S: endocrine manifestations, pathophysiology, treatment (Endocrine Reviews)](https://academic.oup.com/edrv/article/45/5/676/7629683)
- [NSCA: RED-S awareness, identification, management](https://www.nsca.com/education/articles/nsca-coach/relative-energy-deficiency-in-sport-reds-awareness-identification-and-management/)
- [Female Athlete Triad / RED-S nutritional management](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10857508/)
- [Low RED-S knowledge among clinicians](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11235195/)
