# LocaleKit implementation plan

Final fallback/validation decisions: preserve the longest known language/script
prefix before extensions, retain the Portugal override through an intervening
script, then use language fallback; no deprecated-language alias mapping. Catalog
text selection uses exact tag, then the explicitly requested region through an
optional script (before extensions), then matching CLDR rule key, then language, then
source language. Run independent native ICU comparison in addition to the pinned
CLDR Decimal interpreter; document ICU/CLDR version differences rather than
treating a shared generator/interpreter grammar as independent proof.

Independent ICU execution was completed in the Opus final audit, recorded in
tests/feature_gaps_audit/opus-locale-final.md: 157,215 cases against ICU 78.3/CLDR 48,
with differences confined to locales whose rules changed in pinned CLDR 49.
The routine verifier remains the version-matched Decimal/sample suite; the ICU
comparison is separate audit evidence and is not claimed as a routine verifier step.

Final catalog correction: prefer a compatible bare-language translation before
regional siblings. Same-language fallbacks require identical pinned predicates
for the message kind; missing ordinal entries use root other-only. This preserves
Konkani and Portuguese ordinal fallback while preventing Portugal/Brazil cardinal
rule changes. PO imports preserve untouched implicit other fallbacks. The generator
checks its public version literal against the pinned data manifest.

Implement independent portable CLDR 49 cardinal/ordinal category selection for all locale rule entries in the pinned official CLDR JSON. Use exact signed ASCII decimal strings (18 integer + 18 fractional digits) so visible zeros and CLDR `i,v,w,f,t,n` semantics survive without floating-point approximation. Ordinary decimals set `c=e=0`; compact/exponent input rejects. Return standard CLDR category names, validate unsupported locale/number input, and preserve the Portuguese Portugal override before language fallback.

Expose `CardinalPlural`, `OrdinalPlural`, `PluralRuleLocale`, `PluralCLDRVersion`, and a bounded `PluralMessage` with explicit locale and category fallback. Generate straight-line predicates from pinned CLDR grammar; retain source JSON, hashes, license, generator and reproducibility check. Add a companion plural catalog workflow if feasible, keeping existing schema-1 singular generation/PO behavior unchanged.

Risks: exact CLDR integer-membership semantics for `n` versus fractions, preservation of visible trailing zeros, locale fallback/aliases, very large decimal operands, message expansion bounds, generated-code compiler costs, consistency of message language and category locale. Full locale formatting, collation, calendars, and compact-number exponents are outside this version.

Validate actual Buoy compiler JSON for every authored `.bui` and all six targets; compile/run a fresh sealed consumer. Check CLDR published samples plus independent Decimal oracle over deterministic random values and boundary cases, especially Arabic, Russian, Polish, French, Portuguese and English ordinals. Retain runtime evidence and distinguish cross-target semantic checks from native execution.
