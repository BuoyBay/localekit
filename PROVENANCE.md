# Data provenance

This is an independent implementation of Unicode CLDR plural-rule evaluation.
The pinned source is Unicode CLDR JSON release **49.0.0**, repository
[unicode-org/cldr-json](https://github.com/unicode-org/cldr-json/tree/49.0.0),
`cldr-json/cldr-core/supplemental/plurals.json` and `ordinals.json`.

The unmodified official JSON files are retained in `data/`; SHA-256 fingerprints
are recorded in `data/source.json`. The repository's Unicode license is included
as `resources/LICENSE.txt`. The retained data and generated Buoy predicates support offline use. Updating CLDR requires replacing the pinned source files deliberately,
updating `data/source.json` hashes and regenerating the corresponding predicates.
Check the source release and hashes deliberately when changing those inputs.

The predicates implement the finite relation grammar in those CLDR files.
Category sample annotations remain in the retained source JSON; compact-number
annotations are outside the API.

The exact-decimal operand parser and message wrapper are independently written Buoy code. No CLDR engine implementation or
third-party locale formatting library is copied or linked.
