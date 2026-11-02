# CLDR plural forms

Portable CLDR 49 cardinal and ordinal plural categories over **exact decimal text**.
`Import localekit` after copying `packages/localekit-0.1.0.buipkg` into a consumer's
`packages/` directory. No native library or other package is needed.

`CardinalPlural("en", "1")` is `one`; `CardinalPlural("en", "1.0")` is
`other`. French `0.5` is `one`; Arabic `3.0` is `few` and `3.5` is `other`.
`OrdinalPlural("en", "21")` is `one`, corresponding to `21st`, while `11`
is `other`. Categories are grammatical labels, not translated suffixes.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add localekit --git https://github.com/BuoyBay/localekit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API entrypoints

- `Import localekit`
- `CardinalPlural(locale, numberText)`
- `OrdinalPlural(locale, numberText)`
- `PluralRuleLocale(locale)`
- `PluralCLDRVersion()`

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer localekit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
