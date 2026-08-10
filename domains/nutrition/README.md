# Nutrition Domain

Not yet designed in depth. Placeholder scope based on the vision doc and
data model:

- Logging: manual entry at minimum; photo/barcode/integration are
  enhancements, not v0 requirements — don't let logging friction block
  launch, since the AI coach is only as good as the data that reaches it.
- Targets: `NutritionTarget` should derive from the person's active
  ProgramInstance phase (the knowledge base already encodes fueling
  guidance per modality/phase — e.g. hybrid training's carb ranges in
  `04-hybrid-training.md`, bodybuilding's surplus/deficit logic in
  `02-bodybuilding.md`) rather than being set independently of training.
- Open question: how deep does this go — macro tracking only, or
  micronutrient/meal-timing detail? Depth here is a scope decision, not a
  technical one.
