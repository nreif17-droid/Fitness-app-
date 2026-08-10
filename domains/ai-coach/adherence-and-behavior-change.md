# Adherence & Behavior Change

**Why this file exists:** the training library closes with its most important sentence —
*"The programs are not the variable that determines your outcome. Adherence, sleep, and
food are. A B-grade program executed at 90% adherence beats an A+ program at 60%
adherence, every single time."*

The product currently has deep knowledge about programs and (now) sleep and food. This
file covers the remaining one, which is the actual determinant of whether any user gets
a result — and therefore of whether the product is any good.

---

## The baseline reality

- **Dropout of roughly 30%** is typical in long-term exercise interventions, with
  adherence around or above 50% per FITT component considered a normal expectation.
- **20–30% of people cannot maintain behavior change** after a structured intervention
  ends.
- A fitness app's competition is not other fitness apps. It's the user quietly stopping.

**Design consequence:** every feature should be evaluated against "does this make the
user more likely to still be here in week six," not "is this impressive." A perfectly
periodized program the user abandons in week three has produced nothing.

---

## What the evidence says about what works

**Personalization is the strongest lever.** Programs tailored to an individual's
specific goals and preferences improve adherence. This is fortunate: it's exactly what
this product is built to do. The AI coach's personalization isn't just a performance
feature — it's the adherence mechanism.

**Self-regulatory techniques (goal-setting, self-monitoring) work — but with a
caveat.** They improve adherence in unsupported contexts, and notably **do *not* add
much when strong support is already provided** (e.g. supervised sessions). For a
self-serve app where the AI is the only support, self-monitoring and goal-setting are
doing real work. For the owner's directly-coached clients, the human relationship is
already carrying that load.

**Motivational interviewing and health coaching** show promise for the motivational
side specifically — the tone and style of the coach's conversation is not cosmetic.

**Ramp/run-in design matters.** Adjusting the intervention's early ramp period is one
of the identified levers for reducing dropout. The library's conservative beginner
ramps are adherence design as much as injury prevention.

**Enjoyment matters.** Adding a recreational/enjoyable physical activity component
reduced dropout in intervention research. The most effective program is partly the one
the person likes.

---

## Design principles for this product

### 1. Logging friction is the whole game
Already stated in the vision doc; restated here because it's the highest-stakes UI
decision in the app. **If logging is a chore, the data stops arriving and the coach goes
blind** — at which point the differentiator evaporates and it's just another template
app. Defaults, one-tap confirmations, sensible pre-fills from the prescribed session,
and the ability to log a session in under 15 seconds are core, not polish.

### 2. Never punish a missed day
Streak mechanics that reset to zero are actively harmful for the population most at
risk of dropping out — the person who misses three days and concludes they've failed.
The training library's own rule is the right model: **"A skipped run is nothing. Two
skipped weeks means back up one rung."** Build that attitude into the product's
response to a gap, literally.

### 3. Re-entry has to be graceful
The most valuable moment in this product is a user returning after two weeks away. That
is precisely when a plan that says "you're now in Week 7" is wrong and demoralizing.
The coach should recognize the gap, adjust the plan honestly, and say something that
makes returning feel normal rather than shameful.

### 4. Show the reasoning
`CoachDecisionLog` exists for auditability, but its adherence value is larger: a user
who understands *why* the plan changed trusts it and follows it. "Reduced today's
volume because your last two sessions ran 2 RPE above target and you've logged under
6 h sleep twice" is a coaching relationship. A silently different number is an app.

### 5. Honesty over encouragement
The orchestration doc already says the adaptation loop "most needs to be honest, not
encouraging — a coach that never tells someone to back off isn't a coach." The
adherence literature supports this: credibility sustains long-term engagement in a way
that relentless positivity does not. Users can tell the difference between being
supported and being flattered.

### 6. Early wins, deliberately
Beginners see 15–40% improvement in tested metrics (per the library's own expectation
setting). **Surface that.** The Week 0 baseline test isn't just for programming — it's
the thing that makes week-eight progress visible and real. Make retesting and the
before/after comparison a designed moment, not a buried data point.

### 7. Goal quality is a coaching job
Most stated goals are vague ("get in shape") or outcome-only ("lose 20 lbs"). Behavior
goals ("train three times this week") are more controllable, and controllability drives
persistence. The intake conversation should convert a vague aspiration into both a
meaningful outcome goal *and* the behavior goals that get there — that's what the
`Goal` entity's `description` / `target_metric` split is for.

---

## Where this belongs in the three AI jobs

| Job | Adherence role |
|---|---|
| **Intake / goal clarification** | Goal quality (§7), realistic expectation setting, identifying the constraints that will actually cause dropout (time, equipment, schedule volatility) |
| **Program selection & generation** | Match to preference and constraints, not only to goal. The library's own advice — take the beginner track if in doubt — is adherence-protective |
| **Adaptation** | Graceful re-entry (§3), honest explanation (§4), never-punish (§2), and detecting disengagement early enough to respond to it |

**Disengagement is a signal the coach should act on.** A user whose logging frequency is
falling is the highest-value intervention moment in the product, and it's fully
detectable from data already in the schema. Nothing currently specifies what the coach
does about it — flagged as an open design question rather than assumed.

---

## Open questions

- **What does the coach actually do when it detects disengagement?** Reach out? Reduce
  the plan? Ask what's wrong? Doing nothing is a choice, and probably the wrong one.
- **Notification strategy** — the line between helpful and annoying is where a lot of
  fitness apps lose users. Not designed.
- **Whether adherence data feeds the owner's outreach funnel** — a lapsing self-serve
  user is exactly who might convert to real coaching, but using disengagement data for
  a sales approach has an obvious tone risk. Deserves a deliberate decision, not a
  default.

---

## Sources

- [Exercise adherence and self-regulatory behavior change techniques (Phys-Can RCT secondary analysis)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7493247/)
- [Determinants of dropout and variation in adherence: the STRRIDE randomized trials](https://doi.org/10.1249/TJX.0000000000000190)
- [Exploring exercise adherence and quality of life among veteran, novice, and dropout trainees (Frontiers)](https://www.frontiersin.org/journals/sports-and-active-living/articles/10.3389/fspor.2023.1293535/full)
- [Recreational physical activity improves adherence and dropout in behavioral intervention](https://www.tandfonline.com/doi/full/10.1080/02701367.2021.1893259)
- [Predictors of exercise adherence following cardiac rehabilitation](https://link.springer.com/article/10.1207/s15327558ijbm0401_4)
