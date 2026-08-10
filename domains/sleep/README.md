# Sleep & Recovery Domain

## Knowledge base

The substance lives in `knowledge-base/`, mirroring `domains/training/knowledge-base/`.
This is the grounding the AI coach reasons from for anything sleep- or
recovery-related.

| File | Covers |
|---|---|
| `00-sleep-science.md` | Why sleep outranks everything else, how much by population and training load, the honest state of the sleep-extension evidence, sleep debt and naps, hygiene ranked by real effect size, how training affects sleep |
| `01-recovery-and-readiness.md` | **Governs the autoregulation loop.** HRV's genuine uses and its significant limitations, the full readiness picture ranked by signal-per-friction, signal→action decision rules, and recovery modalities ranked honestly (including why post-lifting cold plunges may be counterproductive) |

## Product scope

- **Logging:** `SleepLog` at minimum — hours plus a 1–10 quality self-report. Manual
  entry must work well on its own; wearable integration is an enhancement.
- **Readiness:** `ReadinessCheckIn` is the table the autoregulation logic reads from.
  The decision rules it feeds are specified in `01-recovery-and-readiness.md` and in
  `domains/training/knowledge-base/00-framework.md`.
- **Free regardless of tier**, per the monetization decision — only program depth is
  paywalled.

## The design principle worth not losing

A readiness system built on one wearable sensor is a novelty. One built on a short
daily self-report plus RPE drift from recent sessions is genuinely useful, needs no
hardware, and is available to every user on day one. Build that first.

## Open questions

- Which wearable integrations, if any, ship at v0.
- Whether readiness is presented as a single composite score (appealing, easy to
  over-trust) or as separate components (more honest, less satisfying).
