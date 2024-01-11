---
description: The profanity filter searches and filters out profane words.
---

# Detect profanities

## Why do you need a profanity filter?

Imagine that you build a digital persona that has two sources of knowledge. It can either access knowledge in the form of text from external resources like Reddit or Twitter, or it uses a generative model. Both of these scenarios have a common feature: you don't have complete control over what the persona will say. Thus, checking the appropriateness is necessary.

Also, some users use foul language when talking to digital personas. If you want your digital persona to identify it and react appropriately, the Profanity Filter can detect those instances. We provide you with the Profanity filter for both kinds of situations.

In summary, the Profanity Filter is a component that searches for and filters out profane words in texts. The text is typically the input message of the user or a response of the digital persona over which the dialogue designer doesn't have full control.

<figure><img src="broken-reference" alt=""><figcaption><p>Profanity filter applied to tweets, Reddit fun facts and output of Generative model</p></figcaption></figure>

## How does the Profanity Filter work?

The Profanity filter searches for profane, hateful or toxic inputs. It recognizes several classes of problems: isProfane, isHate, isHateThreatening, isHarassment, isHarassmentThreatening, isSelfHarm, isSelfHarmIntent, isSelfHarmInstructions, isSexual, isSexualMinors, isViolence, and isViolenceGraphic. The isProfane is the overarching profanity class. Is Profane is true if any of `isHate, isHateThreatening, isHarassment, isHarassmentThreatening, isSexual, isSexualMinors, isViolence and isViolenceGraphic` is detected and none of `isSelfHarm, isSelfHarmIntent, and isSelfHarmInstructions` is detected.

| Class                   | Description                                                                                                                                                                                                                                    |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| isProfane               | Content that contains any hate, harassment, sexual or violence, excluding self harm.                                                                                                                                                           |
| isHate                  | Content that expresses, incites, or promotes hate based on race, gender, ethnicity, religion, nationality, sexual orientation, disability status, or caste. Hateful content aimed at non-protected groups (e.g., chess players) is harassment. |
| isHateThreatening       | Hateful content that also includes violence or serious harm towards the targeted group based on race, gender, ethnicity, religion, nationality, sexual orientation, disability status, or caste.                                               |
| isHarassment            | Content that expresses, incites, or promotes harassing language towards any target.                                                                                                                                                            |
| isHarassmentThreatening | Harassment content that also includes violence or serious harm towards any target.                                                                                                                                                             |
| isSelfHarm              | Content that promotes, encourages, or depicts acts of self-harm, such as suicide, cutting, and eating disorders.                                                                                                                               |
| isSelfHarmIntent        | Content where the speaker expresses that they are engaging or intend to engage in acts of self-harm, such as suicide, cutting, and eating disorders.                                                                                           |
| isSelfHarmInstructions  | Content that encourages performing acts of self-harm, such as suicide, cutting, and eating disorders, or that gives instructions or advice on how to commit such acts.                                                                         |
| isSexual                | Content meant to arouse sexual excitement, such as the description of sexual activity, or that promotes sexual services (excluding sex education and wellness).                                                                                |
| isSexualMinors          | Sexual content that includes an individual who is under 18 years old.                                                                                                                                                                          |
| isViolence              | Content that depicts death, violence, or physical injury.                                                                                                                                                                                      |
| isViolenceGraphic       | Content that depicts death, violence, or physical injury in graphic detail.                                                                                                                                                                    |

## Using the Profanity Filter

The Profanity Filter can be used in two contexts: to detect when **users utter profanity words** in their input and to filter out profanities that the **digital persona might use in the responses**.

### Profanities in the user input

The Profanity Filter checks all user input automatically. If it detects any problem, it saves the corresponding information into a Boolean variable `input.isProfane`, `input.isHate` or other class mentioned above. An example of how to use it in the input node follows:

```kotlin
if (input.isProfane){
    toProfanityHandling
} else {
    toNormalContent
}
```

### Profanities in responses

You can use `analyze()` function to handle profanities in responses of digital persona.

#### The analyze() function

```
profanityFilter.analyze(text: String): Response
```

The `profanityFilter.analyze(text: String): Response` function takes a text as input and returns `Response` object. The response object contains results of the analysis under the following attributes:

```
val response = profanityFilter.analyze("Some text")

val isHate = response.results[0].categories.hate
val isHateThreatening = response.results[0].categories.hateThreatening
val isHarassment = response.results[0].categories.harassment
val isHarassmentThreatening = response.results[0].categories.harassmentThreatening
val isSelfHarm = response.results[0].categories.selfHarm
val isSelfHarmIntent = response.results[0].categories.selfHarmIntent
val isSelfHarmInstructions = response.results[0].categories.selfHarmInstructions
val isSexual = response.results[0].categories.sexual
val isSexualMinors = response.results[0].categories.sexualMinors
val isViolence = response.results[0].categories.violence
val isViolenceGraphic = response.results[0].categories.violenceGraphic
```

To test if the text is problematic:

```kotlin
val text = "We have prepared a new update of Flowstorm! The new version includes many changes that will make working in Flowstorm a lot easier."
val response = profanityFilter.analyze(text)

if (!response.results[0].categories.hate && !response.results[0].categories.harassment){
    toSay
}else{
    toDoNotSay
}
```
