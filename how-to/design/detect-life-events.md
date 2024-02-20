---
description: Recognize and understand significant life events mentioned by users
---

# Detect Life Events

The Life Events Detection Skill is a sophisticated feature designed for digital personas to recognize and understand significant life events mentioned by users in English and Czech. This document provides a comprehensive guide on how to implement and utilize the Life Events Detection Skill in your digital persona's dialogue system.

### Overview

Life events are significant milestones or changes in a person's life that can impact their wellbeing and require acknowledgment or adaptation by digital personas. This skill detects such events in conversation, allowing for more personalized and empathetic interactions.

### How to Detect Life Events

To determine if a life event has been mentioned during a dialogue, use the following conditional check:

```kotlin
if (input.containsEntity("LifeEvent")) {
   toLifeEvent
} else {
    toGoOn
}
```

### Accessing Life Event Attributes

Upon detecting a life event, you can access its attributes as follows:

*   **Time of the Event**:

    ```kotlin
    val time = (input.entity("LifeEvent").value as ai.flowstorm.core.model.LifeEvent).time
    ```
*   **Event Seriousness**:

    ```kotlin
    val eventSeriousness = (input.entity("LifeEvent").value as ai.flowstorm.core.model.LifeEvent).eventSeriousness
    ```

### Life Event Attributes

Each life event is characterized by the following attributes:

* `category`: The category of life event (e.g., Financial Wellbeing).
* `domain`: The domain to which the life event belongs (e.g., Financial, Health).
* `acknowledgement`: Whether the event requires acknowledgment.
* `socialReadjustment`: The degree of social readjustment required by the event. (0-100)
* `eventSeriousness`: The seriousness of the event (Low, Medium, High).
* `valence`: The general emotional valence of the event. (0-5)
* `subjectiveValence`: The subjective emotional valence (Negative, Ambiguous, Positive).
* `selfDisclosureIntimacy`: The level of self-disclosure intimacy (Low, Medium, High).
* `time`: The time frame of the event (Past, Present, Future).

#### Example Life Events

```kotlin
HOME_PURCHASE(LifeEventCategory.FINANCIAL_WELLBEING, Domain.FINANCIAL, true, 60.79f, EventSeriousness.MEDIUM, 4.37f, SubjectiveValence.POSITIVE, SelfDisclosureIntimacy.LOW),
CAR_PURCHASE(LifeEventCategory.FINANCIAL_WELLBEING, Domain.FINANCIAL, true, 33.76f, EventSeriousness.LOW, 4.13f, SubjectiveValence.POSITIVE, SelfDisclosureIntimacy.LOW),
```
