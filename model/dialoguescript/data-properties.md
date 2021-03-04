---
description: Data properties are declared in initialisation code of dialogue model.
---

# Data Properties

First, define some data class to play with

```kotlin
data class Country(val name: String, val region: String)
```

## Static definition

You can define your data inside initialisation code

```kotlin
val countries = listOf(Country("Germany", "Europe"), Country("United States", "North America"))
```

## Loading from external source

To load data into model data property from external source, use delegate `loader`.

Example of loading data from external URL providing resource of type JSON with known structure defined as `data class`

```kotlin
val countries by loader<List<Country>>("https://core.flowstorm.ai/file/assets/data/CountryByRegionList.json")
```

If you don't know the exact structure, you can work with data as with `Map` objects

```kotlin
val countries by loader<List<Map<String, Any>>>("https://core.flowstorm.ai/file/assets/data/CountryByRegionList.json")
```

You can also load lists of primitive values, e.g. of type `String`

```kotlin
val countryNames by loader<List<String>>("hhttps://core.flowstorm.ai/file/assets/data/countries.json")
```

You can also use [File Assets](../../app/space/design/file-assets.md) of type JSON. Just upload your file to it and get asset URL from its detail.

