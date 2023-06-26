---
description: Detect emotions that user mentioned in conversation
---

# Detect Mentioned Emotions

This documentation will guide you in identifying emotions verbalized in conversations. It provides an overview of how to use the emotion-related classes.

Note that we do not perform emotion detection, but rather identify emotions that users mention in their conversations. For instance, if a user says "I feel happy," we detect the word "happy" as an emotion.

### Overview

Flowstorm allows you to detect various emotions within text using a comprehensive list of predefined emotions. These emotions are hierarchically structured into emotion classes and subclasses. You can access the detected emotion using a dedicated function provided in the `Input` class.

### Examples of Usage

The `Input` class provides the following two functions to access the detected emotions:

Use the `input.entity()` function to access the first detected emotion of a specific type:

```kotlin
val emotion = input.entity("Emotion")
```

Use the `input.entities()` function to access all the detected emotions of a specific type:

```kotlin
val emotions = input.entities("Emotion")
```

You can then use the detected emotion to fine-tune the conversation flow or generate more context-aware responses. For example, by taking the detected emotion into account, an AI conversational agent might respond positively to joyful emotions:

```kotlin
val emotion = input.entity("Emotion")
if (emotion.subclass == EmotionSubclass.JOY) {
    toRespondPositively
}
```

Or, the agent could provide comfort if detecting sadness:

```kotlin
val emotion = input.entity("Emotion")
if (emotion.subclass == EmotionSubclass.SADNESS) {
    toProvideComfortingWords
}
```

To detect an emotion and access its properties, follow this example:

First, ensure that the conversation context is set up in Flowstorm. Then, use the `Input` class to detect the emotion from user input.

```kotlin
val detectedEmotion = input.entity("Emotion")
```

After detecting the emotion, access its emotion class, noun, adjective, arousal, and valence using the emotion object's properties:

```kotlin
// Access emotion class
val emotionClass = detectedEmotion.emotionSubclass.emotionClass

// Access noun
val emotionNoun = detectedEmotion.noun

// Access adjective
val emotionAdjective = detectedEmotion.adjective

// Access arousal
val emotionArousal = detectedEmotion.arousal

// Access valence
val emotionValence = detectedEmotion.emotionSubclass.valence
```

Now, you have access to the detected emotion's properties, which can be used to guide the conversation flow, generate context-aware responses, or for any other purpose that requires emotion understanding:

```kotlin
logger.info("Emotion Class: $emotionClass")
logger.info("Emotion Noun: $emotionNoun")
logger.info("Emotion Adjective: $emotionAdjective")
logger.info("Emotion Arousal: $emotionArousal")
logger.info("Emotion Valence: $emotionValence")
```

### Emotions in Flowstorm

Flowstorm offers a broad range of emotions in the form of an enum type called `Emotion`. These emotion types consist of various attributes such as:

* Noun
* Adjective
* Emotion subclass
* Arousal type
* Valence numeric value
* Arousal numeric value

#### Emotion Types and Descriptions

Each emotion enums holds a noun, an adjective, and an emotion subclass base, such as:

| Emotion      | Noun         | Adjective | Subclass   | Arousal | Valence Numeric | Arousal Numeric |
| ------------ | ------------ | --------- | ---------- | ------- | --------------- | --------------- |
| ALL\_RIGHT   | all right    | all right | JOY        | NORMAL  | -               | -               |
| AMAZINGNESS  | amazingness  | amazing   | JOY        | HIGH    | 7.72            | 6.05            |
| AMUSEMENT    | amusement    | amused    | AMUSEMENT  | HIGH    | 7.00            | 4.82            |
| AT\_EASE     | easiness     | at ease   | RELIEF     | NORMAL  | -               | -               |
| AWE          | awe          | awed      | ADMIRATION | HIGH    | 6.85            | 3.83            |
| AWESOMENESS  | awesomeness  | awesome   | EXCITEMENT | HIGH    | 7.86            | 6.05            |
| BETTER\_MOOD | better mood  | better    | RELIEF     | NORMAL  | -               | -               |
| BLESSEDNESS  | blessedness  | blessed   | GRATITUDE  | NORMAL  | 7.50            | 3.55            |
| BLISS        | bliss        | blissed   | JOY        | HIGH    | 7.62            | 4.13            |
| BLISSFULNESS | blissfulness | blissful  | JOY        | HIGH    | 7.67            | 3.74            |

#### Emotion Subclasses

Each emotion comes with an assigned emotion subclass that represents the emotion's broader context:

| Emotion Subclass | Emotion Class | Valence  |
| ---------------- | ------------- | -------- |
| ADMIRATION       | JOY           | POSITIVE |
| APPROVAL         | JOY           | POSITIVE |
| GRATITUDE        | JOY           | POSITIVE |
| AMUSEMENT        | JOY           | POSITIVE |
| PRIDE            | JOY           | POSITIVE |
| OPTIMISM         | JOY           | POSITIVE |
| JOY              | JOY           | POSITIVE |
| EXCITEMENT       | JOY           | POSITIVE |
| RELIEF           | JOY           | POSITIVE |
| LOVE             | LOVE          | POSITIVE |

#### Emotion Classes

The Emotion Class serves as the higher-level categorization for specific emotions within the Flowstorm framework and helps to further group emotions into broader contexts. In the following table, you'll find various Emotion Classes with their respective values:

| Emotion Class |
| ------------- |
| UNSPECIFIED   |
| LOVE          |
| JOY           |
| SADNESS       |
| ANGER         |
| FEAR          |
| DISGUST       |
| SURPRISE      |
| SHAME         |
| GUILT         |

#### Arousal and Valence

`Arousal` is an enum that describes how intense an emotion is, and `Valence` is an enum that describes whether an emotion is positive, negative, or ambiguous:

| Arousal     | Valence     |
| ----------- | ----------- |
| UNSPECIFIED | UNSPECIFIED |
| LOW         | NEGATIVE    |
| NORMAL      | AMBIGUOUS   |
| HIGH        | POSITIVE    |

