---
description: Detect entities by GPT
---

# Detect

The `llm.detect` function is used for detecting entities in a given text input using GPT. It takes various parameters to help generate the desired output by training the GPT model with examples provided.

### Function Signature

```kotlin
fun detect(
    input: String,
    prompt: String = "Extract following entities from the provided text, if the text contains them.",
    entityDescription: String,
    examples: List<Pair<String, List<String>>> = listOf(),
    config: LLMConfig = LLMConfig()
): List<String>
```

### Parameters

* `input` (String): The input text that needs to be analyzed to detect entities.
* `prompt` (String): The custom prompt to initiate the detection process.
* `entityDescription` (String): The description of entities to detect.
* `examples` (List\<Pair\<String, List>>): A list of examples in the form of pairs. Each pair contains a text input and a list of entity outputs.
* `config` (LLMConfig): The [configuration object](https://docs.flowstorm.ai/how-to/design/use-gpt/complete) for controlling how GPT generates the output.

### Return Value

The function returns a List of Strings, containing the detected entities.

### Usage Example:

#### In the dialogue function node:

```kotlin
val ent = llm.detect(
    input = input.transcript.text,
    prompt = "Extract following entities from the provided text, if the text contains them."
    entityDescription = "emotions that the speaker is feeling",
    examples = listOf(
        Pair("I was sad and than I was angry", listOf("sad", "angry")),
        Pair("I was so excited to see my friend again this year. At first I was really nervous because I changed a lot and I was unsure of how he would react, but then when I saw him, I actually just felt so relieved when he smiled at me and we hugged.", listOf("excited", "nervous", "unsure", "relieved"))
    )
)
```

In the example above, the `llm.detect` function will analyze the input text to detect and return the emotions felt by the speaker.
