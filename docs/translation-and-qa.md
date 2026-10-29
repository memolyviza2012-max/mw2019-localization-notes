# Translation data and QA

## Suggested working format

Use UTF-8 CSV with stable identifiers and preserved original source text. See the [synthetic example](../examples/translation-template.csv); its keys and strings are invented for illustration and are not extracted game content.

Suggested columns:

| Column | Purpose |
|---|---|
| `key` | Native localization identifier, unchanged |
| `source` | Exact original text for matching and review |
| `translation` | Your translated text |
| `context` | Menu, dialogue, cinematic, etc. |
| `file_path` | Source asset route; not a player's absolute path |

Retain distinct rows when the same key has different original source text. Preserve route metadata separately if an entry appears in several assets. Do not deduplicate by source text alone.

This CSV is an editorial format. It is not directly loadable by the stock game and is not an importer or SDK.

## Preserve executable text structure

Keep parameter substitutions, actual format tokens, controller symbols, color codes, markup, newlines and escaping intact. Examples can include `&&1`, `%s`, `%d` and `^2`; inspect the actual source instead of assuming every percent sign is a placeholder. Preserve empty entries where the engine relies on them.

Keep subtitle movie identifiers, cue identity, start/end timing and nontext columns unchanged. Subtitle tables are a separate content route from ordinary Localize entries.

## Fonts

For Turkish, check at least `ç Ç ğ Ğ ı İ ö Ö ş Ş ü Ü` in every relevant font face and UI context. Do not assume regular and bold share glyph coverage, or that uppercasing respects Turkish casing.

Our Thai project uses paired text mapping and font changes because Thai glyph shaping and placement require additional work. Do not copy that mapping into Turkish text without a demonstrated need. If changing fonts, preserve original Latin and controller/icon glyphs, metrics and required fallback behavior.

## Minimum useful test progression

1. Confirm your executable and asset layouts independently. Our Xbox implementation is not compatible with the described IW8 offline setup.
2. Validate one menu entry and one timed subtitle in the actual game.
3. Close and reopen the game several times; compare the same menu and difficulty/mission names.
4. Test gameplay dialogue and cinematics across mission/scene transitions and asset reloads.
5. Test death, checkpoint reload and returning to the main menu.
6. Compare clean and modded performance on the same PC and settings. Record actual gameplay observations separately from synthetic fixtures.
7. Test installation, upgrades and uninstall in isolated file fixtures, preserving backups and unrelated files.

Keep evidence categories separate: compiled data, fixture pass, developer observation and player report. Neither a clean log nor a complete CSV proves that all translated text is found and consumed by the game's rendering path.
