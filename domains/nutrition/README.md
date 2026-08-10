# Nutrition Domain

## Knowledge base

The substance lives in `knowledge-base/`, mirroring the structure of
`domains/training/knowledge-base/`. This is the grounding the AI coach must reason
from for anything nutrition-related — same rule as training: **if a nutrition
prescription doesn't reference these files, that's a bug, not a shortcut.**

| File | Covers |
|---|---|
| `00-framework.md` | The hierarchy of what matters, energy balance, protein, carbs/fat, hydration, timing, and how targets derive from the active training phase |
| `01-body-composition.md` | Fat loss / muscle gain / recomposition rates, metabolic adaptation, diet breaks, honest measurement, the eating-disorder boundary |
| `02-supplements.md` | Evidence tiers (creatine, caffeine, protein, beta-alanine on top), what to ignore, third-party testing |
| `03-safety-and-scope.md` | **Read before building anything that outputs nutrition advice.** Scope-of-practice limits for a CPT, mandatory refusal/referral categories, RED-S, required product behaviors |

## Product scope

- **Logging:** manual entry at minimum; photo/barcode/integration are enhancements, not
  v0 requirements — don't let logging friction block launch, since the AI coach is only
  as good as the data that reaches it.
- **Targets:** `NutritionTarget` should derive from the person's active ProgramInstance
  phase (see the phase table in `00-framework.md`), never be set independently of
  training.
- **Free regardless of tier.** Per the monetization decision, nutrition logging is not
  paywalled — only program depth is.

## Open questions

- **The scope-of-practice posture is undecided and blocking.** See
  `03-safety-and-scope.md` §1 — whether the app generates individualized nutrition
  targets at all, and under what framing, needs legal review rather than an
  engineering default. It is genuinely viable to ship v0 with training + tracking and
  no generated nutrition prescriptions.
- How deep does tracking go — macro tracking only, or micronutrient/meal-timing detail?
  A scope decision, not a technical one.
