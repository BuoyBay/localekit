# LocaleKit implementation plan

Final fallback/validation decisions: preserve the longest known language/script
prefix before extensions, retain the Portugal override through an intervening
script, then use language fallback; no deprecated-language alias mapping. Catalog
text selection uses exact tag, then the explicitly requested region through an
optional script (before extensions), then matching CLDR rule key, then language, then
source language. CLDR rule-version differences must remain explicit when changing pinned data.

Final catalog correction: prefer a compatible bare-language translation before
regional siblings. Same-language fallbacks require identical pinned predicates
for the message kind; missing ordinal entries use root other-only. This preserves
Konkani and Portuguese ordinal fallback while preventing Portugal/Brazil cardinal
rule changes. PO imports preserve untouched implicit other fallbacks. The generator
checks its public version literal against the pinned data manifest.

Implement independent portable CLDR 49 cardinal/ordinal category selection for all locale rule entries in the pinned official CLDR JSON. Use exact signed ASCII decimal strings (18 integer + 18 fractional digits) so visible zeros and CLDR `i,v,w,f,t,n` semantics survive without floating-point approximation. Ordinary decimals set `c=e=0`; compact/exponent input rejects. Return standard CLDR category names, validate unsupported locale/number input, and preserve the Portuguese Portugal override before language fallback.

Expose `CardinalPlural`, `OrdinalPlural`, `PluralRuleLocale`, `PluralCLDRVersion`, and a bounded `PluralMessage` with explicit locale and category fallback. Generate straight-line predicates from pinned CLDR grammar; retain source JSON, hashes and its license. Add a companion plural catalog workflow if feasible, keeping existing schema-1 singular generation/PO behavior unchanged.

Risks: exact CLDR integer-membership semantics for `n` versus fractions, preservation of visible trailing zeros, locale fallback/aliases, very large decimal operands, message expansion bounds, generated-code compiler costs, consistency of message language and category locale. Full locale formatting, collation, calendars, and compact-number exponents are outside this version.
