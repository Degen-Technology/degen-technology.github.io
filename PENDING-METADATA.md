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
