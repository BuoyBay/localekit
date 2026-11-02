# Explicit-locale plural catalog data

[catalog.json](catalog.json) illustrates a schema-2 message catalog with explicit
source language, translation languages and cardinal/ordinal plural forms. A host
application can use these fields to construct LocaleKit messages; the runtime
package itself operates on explicit locale/category/text values rather than
loading a JSON catalog or gettext files.

## Fields in the sample

- `source_language` identifies the language of source text.
- Singular messages keep one `default` string and per-language text/source/state.
- Plural messages use category/text `default` objects with required `other`.
- `ordinal: true` selects ordinal rules; omission selects cardinal rules.
- Translations carry category `forms`, their source-category snapshot and review state.

Preserve exact decimal number text for CLDR operands and keep the locale of the
selected text explicit. A missing translated category can fall back to that
language's `other`; selecting another language requires applying that language's
rules. `pt` and `pt-PT` may share ordinal rules while differing in cardinal rules.
No likely-subtag or script-to-region inference is implied.

See the [package API](../../docs/API.md) for `PluralMessage`, locale fallback,
input bounds and supported categories.
