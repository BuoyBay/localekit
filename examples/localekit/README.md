# Explicit-locale plural catalog

The companion `tools/localekit_catalog.py` accepts **schema 2** catalogs, extending
the existing schema-1 singular message shape. Existing `buoy-dev l10n` commands,
schema-1 catalogs and generated singular modules remain unchanged.

`generate` always runs real compiler JSON checking against a freshly sealed
LocaleKit dependency before writing. Repeated `--target` selects compiler targets;
the default checks the host. `--check` detects output drift without writing.
`BUOY` and `BPM_PACKAGER` override compiler/sealer. Output must differ from the
input catalog. No translations or catalog files are overwritten implicitly.

Ordinary singular messages retain `default` text and translations with
`text`/`source`/`state`. Plural messages instead have `default` category/text
objects requiring `other`, optional Boolean `ordinal`, and translations containing
`forms`, full source-category `source` snapshot and review `state`. Optional Boolean
`source_ordinal` records the grammatical kind reviewed (omission means cardinal).
Changing a message between cardinal and ordinal makes translations stale until
reviewed with the corresponding source kind. All category
names are lower-case CLDR names. Only current approved translations are generated.

A plural ID `FileCount` produces `FileCountPlural(locale, numberText)`. Import the
generated module alongside the sealed LocaleKit package. The function first finds
an approved translation using exact tag, then an explicitly requested region
(skipping a script and ignoring extensions), then a bare-language translation,
then matching CLDR rule key, then another same-language tag. Non-exact language
fallbacks require identical pinned cardinal or ordinal predicates for this message's
kind. Thus `pt` and `pt-PT` can share ordinal translations, while their different
cardinal rules prevent that fallback; `kok` and `kok-Latn` remain compatible.
If no compatible translation exists, the source language is used. A bare-language
translation takes priority over a regional sibling; when only several compatible
regions remain, their declaration order selects the text. It selects plural rules using **the language of the returned text**,
so a French request with no translation safely uses English categories for English
fallback text. The `other` form handles a category omitted within that language.
No likely-subtag or script-to-region inference is performed: for example,
`zh-Hant` does not imply `zh-TW`. Declared catalog translation tags retain the
existing singular tool's language/optional-region shape. Generated names that
collide with LocaleKit public or private top-level names reject before compiling.
The original number text replaces literal `{count}`. Singular message IDs still
produce native Buoy `Localization` declarations with explicit-override calls.

## PO compatibility

The bridge flattens each plural message into six ordinary schema-1 messages with
stable contexts `FileCount__plural_zero` through `FileCount__plural_other`.
Missing source categories use source `other`; missing translated categories use
that translation's `other`. `notes` include the CLDR category. The existing strict
PO implementation handles review flags, source snapshots, duplicate/unknown IDs,
complete-message-set validation and intentional empty strings. A changed source
category set or ordinal kind marks all flattened translations stale.
PO comments identify cardinal/ordinal kind and list stale source/kind translations
so review does not depend on unchanged category text alone.

This deliberately uses **category contexts**, not gettext `msgid_plural` and
locale-dependent `msgstr[N]` indexes. Indexed plural syntax is rejected. Editors
must preserve contexts and the `buoy-empty` flag. New plural translations must
populate all six contexts to import; partial category approval rejects. An import
with all plural `msgstr` entries untranslated preserves the existing translation
and its original source snapshot. Exported
existing translations already include all six fallback-expanded forms. Keep
translated `other` current because it is the fallback for missing categories.
On re-import, a previously omitted category whose exported text still equals
the old `other` stays omitted. Editing `other` therefore updates its implicit
fallbacks. Previously explicit categories and separately edited categories remain
explicit. New translations still require all six contexts.
On the first import, categories with text identical to the incoming `other` also
remain implicit. To retain an identical string as a separately editable category,
declare that category explicitly in the JSON catalog.

The tool handles catalog source and build-time generation; it adds no runtime
gettext dependency. Its singular portions can be processed by the existing
schema-1 workflow after `flatten`. It is a companion command, not yet a subcommand
of the native `buoy-dev` executable.

Test suites and fixtures are maintainer-local and are not part of this public repository.
