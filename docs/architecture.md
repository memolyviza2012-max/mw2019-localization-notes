# Architecture and lessons

These notes describe our Xbox PC implementation at a general level. They are not an implementation recipe for a different executable.

## Separate forwarding from localization

```text
Game graphics calls -> DXGI proxy -> genuine Windows System32 DXGI

Loaded game localization assets
  -> identify key, original text and asset context
  -> validate current asset ownership and source
  -> publish matching translated text

Cinematic subtitle tables -> validate movie/cue/time/source -> translated text
Thai text mapping <-> matching font glyphs
```

Text is displayed by the game's own UI/subtitle rendering paths. DXGI forwarding is a loading mechanism, not the translation mechanism.

## DXGI compatibility lesson

An earlier loader required every export name in its forwarding list to exist before any forwarding could proceed. One supplied Windows report lacked `DXGIDisableVBlankVirtualization`, and the loader repeatedly stopped at DXGI initialization even though that name had not been requested.

The corrected loader resolves available genuine exports and validates the particular export when it is actually requested. An absent, requested export must fail explicitly rather than jump through a null address or return fabricated success. Do not copy private or undocumented export names as universally available Windows APIs. Forwarding must preserve the calling convention and original arguments.

The actual corrected DLL passed real `CreateDXGIFactory1` and `CreateDXGIFactory2` forwarding tests and a controlled fixture missing the same export as the supplied report. This establishes a forwarding compatibility result, not full gameplay compatibility.

## Localization lifetime and matching

- Match native keys and original text; preserve distinct source variants for a key.
- Confirm the asset still owns the value being replaced. Cached addresses can be reused or reset during loading.
- Multiple native instances may exist for one key. Updating a single lookup result is not necessarily sufficient.
- Preserve published replacement buffers while engine readers may still reference them. Do not free them merely because a table was refreshed.
- Subtitle tables may be per-scene, reordered or present in multiple copies. Validate their schema and match cue identity, movie, timing and source, rather than assuming one fixed global row order.

These are design concerns, not proof that our current runtime solves every instance or load path. Coverage remains incomplete in some later Campaign situations.

## Performance lessons

Avoid unbounded memory scans and per-string heavyweight memory queries in frequently executed paths. Use bounded work, validated caches and a refresh strategy that still reaches newly loaded assets promptly. Prioritizing only menus can leave dialogue untranslated; scanning too slowly can leave assets in English for their entire useful lifetime.

Our earlier full-pool redesign passed synthetic fixtures but performed poorly in actual language coverage because discovery was too slow. A synthetic worker timing is not game FPS and does not demonstrate absence of stutter. Validate both readiness latency and actual gameplay behavior.

## Crash interpretation

One report with missing DXGI exports and another report faulting in another loaded DLL are different observations. A faulting-module name alone does not prove the root cause or exclude an interaction with the mod. Compare clean and modded launches under the same conditions and change one factor at a time.

Keep diagnostic collection local and selective. Do not publish player reports, full filesystem paths, account identifiers or machine details without the player's permission. These public notes intentionally omit those records.
