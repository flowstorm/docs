---
description: Detect people that user mentioned in conversation
---

# Detect Mentioned People

This documentation will guide you in identifying people and their roles verbalized in conversations. Note that we do not perform detection of specific people, but rather identify their family role or occupation. For instance, if a user says "I like my mother," we detect the word "mother" as a person.

{% hint style="info" %}
Detection of mentioned people is available in 🇺🇸 English, 🇨🇿 Czech, 🇩🇪 German, and 🇪🇸 Spanish dialogues
{% endhint %}

#### Overview <a href="#overview" id="overview"></a>

We allow you to detect various people and their roles within text using a comprehensive list of predefined people. These people are hierarchically structured into people and people roles. You can access the detected people using a dedicated function provided in the `Input` class.

#### Examples of Usage <a href="#examples-of-usage" id="examples-of-usage"></a>

The `Input` class provides the following two functions to access the detected people:

Use the `input.entity()` function to access the first detected person of a specific type:

```kotlin
val emotion = input.entity("People")
```

Use the `input.entities()` function to access all the detected people:

```kotlin
val emotions = input.entities("People")
```

You can also access attributes of person:

```kotlin
val emotion = (input.entity("People").value as ai.flowstorm.core.model.Person)
```

You can also branch the dialogue according to the detected person:

```kotlin
val person = (input.entity("People").value as ai.flowstorm.core.model.Person)
if (person == Person.MOTHER) {
    toMotherHandling
} else {
    toOrdinaryDialogue
}
```

### Structure of People Entities

The People entities consist of `Person`, `PersonRole` and `PersonArea` entities. The structure of the `Person` entity, `PersonRole` entity, and `PersonArea` is designed to represent various aspects of human relationships and roles.&#x20;

The `Person` entity is an enumeration that represents specific individuals or roles. Each `Person` is associated with a `PersonRole`, which further categorizes the individual into broader relationship categories. There are two special `Person` entities:

* **I:** Refers to the user themselves.
* **ROLE\_ONLY:** Used when a role is mentioned (parent, spouse, in law...) without specifying whether it's a mother, father, wife, husband, mother in law, father in law etc.

`PersonRole` is an entity that groups `Person` entities into broader categories based on their role or relationship. Each `PersonRole` is associated with a `PersonArea`, which represents the broader context or environment of the relationship (e.g., Family, Work).

`PersonArea` is an enumeration representing the context or environment where a person or role is typically found.

To access the `PersonArea` of a `Person`, you first determine the `PersonRole` associated with that `Person`, and then access the `PersonArea` linked to that `PersonRole`.

```kotlin
val person = (input.entity("People").value as ai.flowstorm.core.model.Person)
val role = person.role
val area = role.area

logger.info(person)
logger.info(role)
logger.info(area)
```

In this example, if there is `Person.FATHER` in `input`,  accessing the `role` of `Person.FATHER` will lead to `PersonRole.PARENT`, and accessing the `area` of `PersonRole.PARENT` will lead to `PersonArea.FAMILY`.
