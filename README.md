# MW2019 Campaign Localization Notes

General implementation notes from NodNuat Translator's Thai localization for **Call of Duty: Modern Warfare (2019), Xbox app / Microsoft Store PC**.

This repository is documentation for independent localization developers. **It is not a working mod, a public SDK, or the source code of the shipped runtime.** It contains no game assets, game text, executable offsets, runtime DLLs, account data or player reports.

## Compatibility and project scope

Our implementation targets the Xbox PC package **1.0.9.0** we developed against. Package version alone is not sufficient to establish binary compatibility.

**Our files and implementation are not interchangeable with the "IW8 1.38 Offline v0.3" setup described to us.** Do not install our Xbox-targeted DLL into that setup. We do not provide a compatible implementation for it, Steam or Battle.net.

We deliberately restrict our distributed project to the Xbox PC version. We do not want to expand support to other versions and potentially contribute to players receiving account bans through mod use. This is a project scope and risk-management decision, **not evidence that Xbox PC is exempt from bans or that our mod guarantees account safety**.

The mod is intended for Campaign localization. Our current implementation does not block network connections or lock game modes. We recommend uninstalling before Multiplayer or Co-op; that recommendation is not a guarantee against enforcement.

## What the implementation does

The DXGI proxy forwards graphics API calls to the genuine Windows System32 DXGI library. It does not translate text or draw a translation overlay. A separate runtime component locates loaded localization assets and substitutes matching translated strings. Cinematic subtitle tables and Thai font support are handled separately.

Read [Architecture and lessons](docs/architecture.md) and [Translation data and QA](docs/translation-and-qa.md) for the useful general principles and known limits.

## Current evidence and limitations

As of 29 October 2026, the user reported improved operation of the startup compatibility test and authorized release 1.1.1 using that same DLL. This does not establish complete Campaign translation coverage, consistent behavior on all PCs, measured FPS or complete death/checkpoint QA. Some later Campaign content can still alternate between translated and English text.

The packaged payload contains 27,263 Localize entries, 467 cinematic subtitle rows and five paired fonts. These are data counts, not a claim that every string in the engine has been extracted or every row is displayed in Thai during gameplay.

## Independent projects

Keep your implementation independent and verify your own executable and asset layouts. For Turkish, first check the game's original fonts for all needed characters before introducing replacement fonts. Start with one menu string and one subtitle, then test asset reloads and mission transitions.

These notes do not document anti-cheat evasion, ban removal, authentication bypasses or protection-disabling procedures. There is no general-purpose native archive rebuilding workflow verified by this MW2019 project; do not substitute recipes from Modern Warfare Remastered, which is a different game and engine revision.

## Reuse and contact

You may quote and adapt the original prose and synthetic CSV example in this repository for your own localization documentation, with attribution to NodNuat Translator and a link back. This permission does not grant rights to the distributed mod, its runtime code, game assets, translations, artwork or third-party material. This is documentation sharing, not an offer of ongoing development support or endorsement of another build.

- Website: https://www.nodnuattranslator.com/
- Facebook: https://www.facebook.com/NodNuatTranslator/

The Thai project uses AI-assisted translation with polishing and language QA through **THub Studio 2.2**. This is an independent fan project and is not affiliated with Activision, Microsoft or the game's developers.
