---
title: "localekit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["PluralMessage"]
dependencies: []
symbols: ["PluralMessage"]
related: ["api.md", "getting-started.md", "index.md"]
---

# localekit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## LocaleKit functions

### PluralRuleLocale

`Public Function PluralRuleLocale(locale As String) As String`

Resolve a supported locale tag to a CLDR rule key, preserving region-specific rules.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `locale` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### CardinalPlural

`Public Function CardinalPlural(locale As String, numberText As String) As String`

Select zero/one/two/few/many/other from an exact signed decimal; compact exponents are zero.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `locale` | `String` | `ByVal` | Required |
| `numberText` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### OrdinalPlural

`Public Function OrdinalPlural(locale As String, numberText As String) As String`

Select an ordinal category, preserving the number's visible decimal precision.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `locale` | `String` | `ByVal` | Required |
| `numberText` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### PluralCLDRVersion

`Public Function PluralCLDRVersion() As String`

No parameters.

#### Returns

| Type |
| --- |
| `String` |

## PluralMessage

`Public Class PluralMessage`

A bounded, explicit-locale message with category fallback and literal {count} substitution.

### Constructor

`Public Sub Constructor(locale As String, otherText As String, ordinal As Boolean = False)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `locale` | `String` | `ByVal` | Required |
| `otherText` | `String` | `ByVal` | Required |
| `ordinal` | `Boolean` | `ByVal` | `False` |

Returns: no value.

### SetForm

`Public Sub SetForm(category As String, text As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `category` | `String` | `ByVal` | Required |
| `text` | `String` | `ByVal` | Required |

Returns: no value.

### Format

`Public Function Format(numberText As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `numberText` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |
