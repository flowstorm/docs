---
description: Detect language of the user input
---

# Detect Language

The Language Detection feature is used to identify the language of the user inputs.

### Accessing Detected Language

To access the detected language in the user's conversation, you can use the `Input` class's `identifiedLanguage` attribute.

Example:

```kotlin
val detectedLang = input.identifiedLanguage
```

### List of supported languages

```
af als am an ar arz as ast av az azb ba bar bcl be bg bh bn bo bpy br bs bxr ca cbk ce ceb ckb co cs cv cy da de diq dsb dty dv el eml en eo es et eu fa fi fr frr fy ga gd gl gn gom gu gv he hi hif hr hsb ht hu hy ia id ie ilo io is it ja jbo jv ka kk km kn ko krc ku kv kw ky la lb lez li lmo lo lrc lt lv mai mg mhr min mk ml mn mr mrj ms mt mwl my myv mzn nah nap nds ne new nl nn no oc or os pa pam pfl pl pms pnb ps pt qu rm ro ru rue sa sah sc scn sco sd sh si sk sl so sq sr su sv sw ta te tg th tk tl tr tt tyv ug uk ur uz vec vep vi vls vo wa war wuu xal xmf yi yo yue zh
```

### Locale Object

The identified language itself is saved in `Input.identifiedLanguage.value` as a `Locale` object, meaning you can use any attribute of the `Locale` objects, such as:

* `language` - Language code in the ISO 639 standard (e.g. "en").
* `displayLanguage` - Human readable language name (e.g. "English").

### Example

```kotlin
val detectedLang = input.identifiedLanguage

if (detectedLang != null && detectedLang.score > 0.8) {
    if (detectedLang.value.language == "en") {
        // Handle English user query
    } else {
        // Handle non-English user query
    }
}
```

In this example, we first check if a language was detected in the user input, and whether the language identifier model is confident enough about the result. If yes, we can take appropriate action based on the user input language.
