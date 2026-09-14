# Pending App Store metadata changes

These are **blocked until each app's next version submission** — `marketingUrl` is a
version-scoped field and Apple locks it once a version is `READY_FOR_SALE`.

Verified 2026-08-31 two independent ways, so this isn't a guess:
- API `PATCH appStoreVersionLocalizations` → `409 STATE_ERROR`,
  *"Attribute 'marketingUrl' cannot be edited at this time"*
- App Store Connect UI → the field renders read-only on the live version and the
  Save button stays disabled (typing into it does nothing)

## Apply at the NEXT version submission for each app

| App | Current marketingUrl | Change to |
|---|---|---|
| Mealite | `https://www.degentechnology.com/` | `https://degen-technology.github.io/mealite/` |
| Lock In | `https://www.degentechnology.com/` | `https://degen-technology.github.io/lockin/` |
| Citadel Champions | `https://www.degentechnology.com/` | `https://degen-technology.github.io/citadel-champions/` |
| Wren | `https://jmarrr.github.io/legal/wren-privacy.html` ⚠️ | `https://degen-technology.github.io/wren/` |
| Labor & Contraction Timer | `https://degentechnology.com` | `https://degen-technology.github.io/contraction-timer/` |

⚠️ **Wren is the one worth fixing first.** Its marketing URL points at a *privacy policy*
page on a **personal** GitHub account (`jmarrr.github.io`), not the company's — wrong content
and wrong owner. Its `supportUrl` (`https://jmarrr.github.io/legal/`) has the same problem.

## How to apply

Once a version record is editable (i.e. a new version exists and is not yet live):

```
PATCH /v1/appStoreVersionLocalizations/{id}
{"data":{"type":"appStoreVersionLocalizations","id":"{id}",
 "attributes":{"marketingUrl":"https://degen-technology.github.io/<slug>/"}}}
```

Then **read the value back** — the PATCH silently no-ops on a locked version, which is how
this whole limitation was found.

All five landing pages are live and verified 200, so the URLs are safe to set the moment a
version opens up.

## Lock In — Keywords (Irene, 2026-09-14)

Same lock: `PATCH keywords` → `409 "Attribute 'keywords' cannot be edited at this time"`
(1.1.7 is READY_FOR_SALE). Rides with Lock In's next version, alongside the marketingUrl above.

Current (99 chars):
`productivity,goal tracker,daily goals,habit tracker,time management,motivation,task manager,planner`

Irene's requested string (98 chars):
`5 tasks,task manager,habit tracker,daily goals,planner,streak,focus mode,time management,checklist`

⚠️ **One premise in the request is incorrect and worth resolving before applying.** Irene wrote
that "5 tasks" is *"not indexed anywhere (not in the App Name, Subtitle, or Keywords)"*. It IS —
the **subtitle is "Only 5 Tasks. No Excuses."** Apple indexes App Name + Subtitle + Keywords as
one pool and explicitly says repeating a term across them does not improve ranking. So:
- `5 tasks` duplicates the subtitle (~8 chars wasted)
- `focus mode` duplicates `Focus` in the app name — only `mode` is new (~6 chars wasted)

If "5 tasks" really ranks 59th, the cause is competition/authority, not missing indexing — so
re-adding it to Keywords will not move it, and costs 14 of 100 characters.

Alternative (92 chars) — same intent, reclaims the wasted space for terms tied to the app's
actual differentiator (Focus Mode blocks apps; competitors there are Freedom/Opal):
`task manager,habit tracker,daily goals,planner,streak,mode,app blocker,screen time,checklist`

**Decision needed from Alan/Irene** before the next Lock In submission.
