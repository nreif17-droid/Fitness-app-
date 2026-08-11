# Fix 01 — Entitlement bypass in `generate-program`

**Status:** written, **not yet applied** (Base44 MCP needs re-authentication).
**Severity:** 🔴 High — revenue loss. Any authenticated user can obtain any premium
program for free.
**App:** Apex Vitality (`6a78de5f6a884d5cd8b7a267`)
**File:** `base44/functions/generate-program/entry.ts`

---

## The vulnerability

`generate-program/entry.ts` accepts a `programId`, looks up the program, and generates
the full 12-week program. **There is no entitlement check anywhere in the function.**

`ProGate.jsx` is purely client-side — it takes an `isPro` boolean and conditionally
renders JSX. That hides the button; it does not protect the endpoint. Anyone who can
open devtools, or call the function endpoint directly with a premium `programId`, gets
the complete paid program.

`WorkoutProgram` RLS `read` is `{}` (fully open), so discovering premium program ids is
trivial.

---

## The fix

Insert an entitlement gate immediately after the program lookup and before the prompt is
built. Uses only fields that already exist in the schema — no migration required.

### Patch

In `base44/functions/generate-program/entry.ts`, find:

```ts
    const program = (programs || []).find((p) => p.id === programId);
    if (!program) {
      return Response.json({ error: "program not found" }, { status: 404 });
    }

    const sport = SPORT_LABELS[program.athlete_type] || program.athlete_type || "general fitness";
```

Replace with:

```ts
    const program = (programs || []).find((p) => p.id === programId);
    if (!program) {
      return Response.json({ error: "program not found" }, { status: 404 });
    }

    // --- Entitlement gate -------------------------------------------------
    // Server-side enforcement. ProGate.jsx is presentation only and must never
    // be the sole barrier to paid content.
    // Fails CLOSED: a program with is_premium unset is treated as premium,
    // because the schema default for is_premium is true.
    const profileForGate = ctx.profile || {};
    const isPremium = program.is_premium !== false;
    const isAdmin = me?.role === "admin";
    const isPro = profileForGate.plan_tier === "pro";
    const ownsProgram =
      Array.isArray(profileForGate.owned_program_ids) &&
      profileForGate.owned_program_ids.includes(programId);

    if (isPremium && !isAdmin && !isPro && !ownsProgram) {
      return Response.json(
        {
          error: "This program requires a purchase or an active Pro plan.",
          code: "entitlement_required",
          program_id: programId,
        },
        { status: 403 },
      );
    }
    // ----------------------------------------------------------------------

    const sport = SPORT_LABELS[program.athlete_type] || program.athlete_type || "general fitness";
```

### Why this placement

- It sits **after** `me` and `ctx` are resolved (both are awaited just above in the
  existing `Promise.all`), so no extra round trips.
- It sits **before** the `InvokeLLM` call, so an unentitled request costs nothing.
- `profileForGate` is used rather than the existing `profile` const because `profile` is
  declared a few lines further down; this avoids reordering existing code.

### Notes on the logic

- **Fail-closed on `is_premium`.** The schema default is `true`, so `undefined` is
  treated as premium. If genuinely free programs exist, they must set
  `is_premium: false` explicitly. Verify the free/intro programs are flagged correctly
  after applying, or free users will be locked out of content they should have.
- **Admins bypass**, so the owner can preview any program.
- **This matches the *current* monetization model** (binary `plan_tier` +
  `owned_program_ids`). It deliberately does **not** implement the decided week-based
  tier model (Free wks 1–4 / $18 wks 1–8 / $45 full / $200 Pro) — that's a larger
  change and conflating the two would make this security fix harder to verify. See
  fix 03.

---

## Secondary hardening: `WorkoutProgram` RLS

Current: `read: {}` — every field of every program readable by anyone authenticated,
including `days[]` (the base-week template, which is sellable content).

**Do not simply lock this down.** Users must read program metadata to browse and buy the
catalog. Locking `read` outright breaks the storefront.

Two workable approaches, in preference order:

1. **Split the entity.** Keep public catalog metadata (title, subtitle, description,
   athlete_type, difficulty, price, cover image, tags, prerequisites) on
   `WorkoutProgram`, and move the sellable payload (`days[]`, `progression`,
   `test_protocol`) to a separate entity with restrictive RLS. Cleanest, but a
   migration.
2. **Serve program detail through a backend function** that applies the same
   entitlement gate as above and strips `days`/`progression` for unentitled users.
   Smaller change; leaves the entity readable but stops the client from ever being the
   thing that decides.

Neither is required to close the primary hole — the server-side gate above stops the
expensive, valuable output from being generated. Treat RLS as follow-up hardening, not
a prerequisite.

---

## Verification after applying

1. **Free user, premium program → 403.** Log in as a non-Pro user who does not own the
   program, call `generate-program` with a premium `programId`. Expect
   `403 entitlement_required`, and confirm no `AIInsight` row was created.
2. **Free user, free program → 200.** Confirm intro/free programs still generate.
   If they 403, they're missing `is_premium: false`.
3. **Pro user → 200** on any program.
4. **Owner of a specific program → 200** for that program, **403** for a different
   premium one.
5. **Admin → 200** on anything.
6. **Direct endpoint call** with a premium id, bypassing the UI entirely — this is the
   actual attack, and the only test that proves the fix. Confirm 403.
