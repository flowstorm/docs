---
description: Detect that user mentioned some negative thought
---

# Detect Negative Thoughts

## Negative Thought Detection

The Negative Thought Detection feature is used to identify negative thoughts in a conversation.&#x20;

### Accessing Detected Negative Thoughts

To access the detected negative thoughts in the user's conversation, you can use the `Input` class's `negativeThought` attribute.

Example:

```kotlin
val detectedThought = input.negativeThought
```

### Enumerating Negative Thoughts

There are several types of negative thoughts that can be detected in the conversation. They are defined in the `NegativeThought` enumeration with the following values:

* ALL\_OR\_NOTHING\_THINKING
* CATASTROPHIZATION
* MIND\_READING\_AND\_FORTUNE\_TELLING
* LABELING
* EMOTIONAL\_REASONING
* OVERGENERALIZATION
* CONTROL\_FALLACY
* MUSTS\_SHOULDS
* SELF\_CRITICISM

### Example

```kotlin
val detectedThought = input.negativeThought

if (detectedThought != null) {
    when (detectedThought.value) {
        NegativeThought.ALL_OR_NOTHING_THINKING -> {
            // Handle All-or-Nothing Thinking
        }
        NegativeThought.CATASTROPHIZATION -> {
            // Handle Catastrophization
        }
        NegativeThought.MIND_READING_AND_FORTUNE_TELLING -> {
            // Handle Mind Reading and Fortune Telling
        }
        // ...
    }
}
```

In this example, we first check if a negative thought is detected in the user's conversation. If yes, we examine which type of negative thought it is and take appropriate action accordingly.
