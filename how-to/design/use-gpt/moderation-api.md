---
description: Check safety of texts
---

# Moderation API

## Moderation

The `Moderation API` is a tool for checking whether a given text is safe. It can help developers identify content that violates safety policies and take appropriate action, such as filtering it out.

### Usage

To use the `Moderation API`, call the `analyze()` method with the input text you want to moderate:

```kotlin
val moderationResponse = moderation.analyze("Your input text here")
```

The `analyze()` method returns a `ModerationResponse` object, which contains information about whether the input text violates any of the following categories:

| Category         | Description                                                                                                                                                     |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| hate             | Content that expresses, incites, or promotes hate based on race, gender, ethnicity, religion, nationality, sexual orientation, disability status, or caste.     |
| hate/threatening | Hateful content that also includes violence or serious harm towards the targeted group.                                                                         |
| self-harm        | Content that promotes, encourages, or depicts acts of self-harm, such as suicide, cutting, and eating disorders.                                                |
| sexual           | Content meant to arouse sexual excitement, such as the description of sexual activity, or that promotes sexual services (excluding sex education and wellness). |
| sexual/minors    | Sexual content that includes an individual who is under 18 years old.                                                                                           |
| violence         | Content that promotes or glorifies violence or celebrates the suffering or humiliation of others.                                                               |
| violence/graphic | Violent content that depicts death, violence, or serious physical injury in extreme graphic detail.                                                             |

#### ModerationResponse

The `ModerationResponse` object contains the following properties:

* `id`: A unique identifier for the moderation response.
* `model`: The model used for the moderation.
* `results`: A list of `Result` objects containing the moderation analysis.

**Result**

The `Result` object contains the following properties:

* `categories`: A `Categories` object containing boolean values that indicate whether the input text violates each category.
* `categoryScores`: A `CategoryScores` object containing the scores for each category. The score is a double value between 0 and 1, where higher scores indicate a higher likelihood that the input text violates the category.
* `flagged`: A boolean value that indicates whether the input text is flagged as violating any of the categories.

**Categories**

The `Categories` object contains the following boolean properties:

* `hate`
* `hateThreatening`
* `selfHarm`
* `sexual`
* `sexualMinors`
* `violence`
* `violenceGraphic`

**CategoryScores**

The `CategoryScores` object contains the following double properties:

* `hate`
* `hateThreatening`
* `selfHarm`
* `sexual`
* `sexualMinors`
* `violence`
* `violenceGraphic`

#### Example

Here's an example of how to use the `Moderation API` to analyze a generated text and output all results:

```kotlin
val response = llm.chat(
    context, 
    numTurns = 5, 
    prompt = "You are digital persona Kai.", 
    personaName = "Kai")
val moderationResponse = moderation.analyze(response)

logger.info("Moderation ID: ${moderationResponse.id}")
logger.info("Model: ${moderationResponse.model}")

var result = moderationResponse.results.first() 
logger.info("Flagged: ${result.flagged}")
logger.info("Hate: ${result.categories.hate} (Score: ${result.categoryScores.hate})")
logger.info("Hate/Threatening: ${result.categories.hateThreatening} (Score: ${result.categoryScores.hateThreatening})")
logger.info("Self-Harm: ${result.categories.selfHarm} (Score: ${result.categoryScores.selfHarm})")
logger.info("Sexual: ${result.categories.sexual} (Score: ${result.categoryScores.sexual})")
logger.info("Sexual/Minors: ${result.categories.sexualMinors} (Score: ${result.categoryScores.sexualMinors})")
logger.info("Violence: ${result.categories.violence} (Score: ${result.categoryScores.violence})")
logger.info("Violence/Graphic: ${result.categories.violenceGraphic} (Score: ${result.categoryScores.violenceGraphic})")
```

Here's an example of how to use the `Moderation API` to branch a dialogue flow:

```kotlin
val response = llm.chat(
    context, 
    numTurns = 5, 
    prompt = "You are digital persona Kai.", 
    personaName = "Kai")
val moderationResponse = moderation.analyze(response) // response is a text we want to analyze
var result = moderationResponse.results.first() // result is a helper function for easier access to flagged

if (result.flagged){
    toHandleNotSafe
} else {
    toSafe
}
```

Here's an example of how to use the `Moderation API` to branch a dialogue flow according to category:

```kotlin
val response = llm.chat(
    context, 
    numTurns = 5, 
    prompt = "You are digital persona Kai.", 
    personaName = "Kai")
val moderationResponse = moderation.analyze(response) // response is a text we want to analyze
var result = moderationResponse.results.first() // result is a helper function for easier access to categories

if (result.categories.hate) {
    toHandleHate
} else if (result.categories.hateThreatening) {
    toHandleHateThreatening
} else if (result.categories.selfHarm) {
    toHandleSelfHarm
} else if (result.categories.sexual) {
    toHandleSexual
} else if (result.categories.sexualMinors) {
    toHandleSexualMinors
} else if (result.categories.violence) {
    toHandleViolence
} else if (result.categories.violenceGraphic) {
    toHandleViolenceGraphic
} else if (result.flagged) {
    toHandleNotSafe // added just to be safe
} else {
    toSafe
}
```
