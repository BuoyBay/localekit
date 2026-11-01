# Data provenance

This is an independent implementation of Unicode CLDR plural-rule evaluation.
The pinned source is Unicode CLDR JSON release **49.0.0**, repository
[unicode-org/cldr-json](https://github.com/unicode-org/cldr-json/tree/49.0.0),
`cldr-json/cldr-core/supplemental/plurals.json` and `ordinals.json`.

The unmodified official JSON files are retained in `data/`; SHA-256 fingerprints
are recorded in `data/source.json`. The repository's Unicode license is included
as `resources/LICENSE.txt`. Ordinary compilation/sealing and verification work
offline. Updating CLDR requires replacing the pinned source files deliberately,
updating `data/source.json` hashes and the generator's release metadata, regenerating,
and rerunning tests. Generation validates the existing pins and each input's CLDR
version before producing output; it never refreshes hashes implicitly.

`tools/generate_localekit_tables.py` parses the finite relation grammar present
in these files and emits Buoy predicates. It fails on unrecognized grammar rather
than guessing. Category sample annotations remain in the source JSON and drive
runtime verification; compact-number annotations are explicitly outside the API.

The exact-decimal operand parser, message wrapper, generator and verification
interpreter are newly written workspace code. No CLDR engine implementation or
third-party locale formatting library is copied or linked.
