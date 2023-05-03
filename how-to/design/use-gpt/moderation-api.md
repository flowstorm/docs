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

Here's an example of how to use the `Moderation` class to analyze a given text:

```kotlin
val inputText = "Some text to analyze"
val moderationResponse = moderation.analyze(inputText)

println("Moderation ID: ${moderationResponse.id}")
println("Model: ${moderationResponse.model}")

var result = moderationResponse.results.first() 
println("Flagged: ${result.flagged}")
println("Hate: ${result.categories.hate} (Score: ${result.categoryScores.hate})")
println("Hate/Threatening: ${result.categories.hateThreatening} (Score: ${result.categoryScores.hateThreatening})")
println("Self-Harm: ${result.categories.selfHarm} (Score: ${result.categoryScores.selfHarm})")
println("Sexual: ${result.categories.sexual} (Score: ${result.categoryScores.sexual})")
println("Sexual/Minors: ${result.categories.sexualMinors} (Score: ${result.categoryScores.sexualMinors})")
println("Violence: ${result.categories.violence} (Score: ${result.categoryScores.violence})")
println("Violence/Graphic: ${result.categories.violenceGraphic} (Score: ${result.categoryScores.violenceGraphic})")

if (result.flagged){
    toHandleNotSafe
} else {
    toSafe
}
```
