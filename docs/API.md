# LocaleKit 0.1.0

Portable CLDR 49 cardinal and ordinal plural categories over **exact decimal text**.
`Import localekit` after copying `packages/localekit-0.1.0.buipkg` into a consumer's
`packages/` directory. No native library or other package is needed.

| API | Behavior |
| --- | --- |
| `CardinalPlural(locale, numberText)` | `zero`, `one`, `two`, `few`, `many` or `other` |
| `OrdinalPlural(locale, numberText)` | CLDR ordinal category; missing ordinal rules inherit root `other` |
| `PluralRuleLocale(locale)` | Validated lower-case rule key with language fallback |
| `PluralCLDRVersion()` | `49` |
| `New PluralMessage(locale, otherText, ordinal = False)` | Explicit-locale category message; `other` is mandatory |
| `message.SetForm(category, text)` | Add/replace a category, including intentional empty strings |
| `message.Format(numberText)` | Choose a form and replace every literal `{count}` with original number text |

`CardinalPlural("en", "1")` is `one`; `CardinalPlural("en", "1.0")` is
`other`. French `0.5` is `one`; Arabic `3.0` is `few` and `3.5` is `other`.
`OrdinalPlural("en", "21")` is `one`, corresponding to `21st`, while `11`
is `other`. Categories are grammatical labels, not translated suffixes.

The source contains all **225 cardinal and 109 ordinal locale entries** in the
pinned JSON. Rule selection tries a full locale key, then the longest known
language/script prefix before extensions, then its language;
`pt-Latn-PT` preserves the Portugal override before falling back. Hyphens and
underscores are accepted, tags are case insensitive, and Unicode extension
subtags do not change plural rules. Unknown languages throw rather than quietly
returning English rules. This is bounded rule-key lookup, not a full BCP 47
canonicalizer: deprecated language aliases are not mapped.

## Numbers and bounds

The grammar is `[+-]digits[.digits]`, with one to 18 integer digits and zero to
18 fractional digits. Whitespace, grouping separators, exponent/compact notation,
non-ASCII digits, NaN and infinities reject. Leading zeros are accepted within
those digit limits. Negative values use absolute-value CLDR operands, while message
substitution preserves their sign. Input strings have a hard 39-scalar limit.

Visible trailing zeros matter: `1`, `1.0`, and `1.00` retain distinct `v`/`w`/`f`/`t`
operands. Every predicate uses exact signed 64-bit integer arithmetic; no decimal
is first rounded through a `Double`. CLDR's `n` integer-range membership excludes
nonintegral values. This ordinary-decimal API sets compact exponent operands
`c=e=0`; it does not evaluate CLDR compact-number samples such as `1c6`.

Messages permit at most one million scalars per form and in the substituted
result. Replacement takes linear time; its temporary result is bounded below six
million scalars before the final one-million-scalar check. Missing categories fall back to `other`; an explicitly
empty form stays empty. There is no interpolation other than literal `{count}`.
Messages are mutable and should be confined to one thread or externally synchronized.
Invalid inputs, categories, locales and bounds throw `InvalidArgumentException`.
No current OS language or locale is read or changed.

## Scope and verification

This package provides plural rules and explicit-locale messages. It does not add
number/date formatting, locale collation, calendars, currency patterns, language
alias canonicalization, ICU message syntax or gettext runtime loading. It does not
change Buoy's built-in `Locale` or `Localization` declarations.

Maintainers run the private validation suite before release. See the repository README for public build instructions.

Regenerate predicates with `python3 tools/generate_localekit_tables.py`; use
`--check` for reproducibility verification. Hand-written source lives in
`data/LocaleKit.bui.in`; generated `modules/LocaleKit.bui` is the compiled artifact.
See [provenance](../PROVENANCE.md).

Verification seals into an isolated temporary directory. Add `--update-package`
to `tests/localekit_verify.py` to explicitly refresh the checked-in package after
successful checks; ordinary validation leaves that archive unchanged.

Maintainers run the private validation suite before release. See the repository README for public build instructions.
