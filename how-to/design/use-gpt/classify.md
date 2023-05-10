---
description: Text Classification using llm.classify with GPT
---

# Classify

Function `llm.classify` enables text classification using GPT (Generative Pre-trained Transformer) models.

### Function Signature

```kotlin
fun classify(
    input: String,
    examples: List<Pair<String, String>>,
    prompt: String = "Classify text.",
    config: LLMConfig = LLMConfig()
): String
```

### Parameters

* `input` (String): The text input that needs to be classified.
* `examples` (List\<Pair\<String, String>>): A list of example pairs, where each pair consists of an input text and its corresponding class label. The first element of pair is input text, the second element is class label.
* `prompt` (String, optional): A custom prompt for the classification task. Default value is "Classify text.".
* `config` (LLMConfig, optional): Configuration object for the GPT model. Uses default configuration if not provided.

### Return Value

* `String`: The predicted class label for the input text.

### Usage

The `llm.classify` function takes a text input, a set of example pairs, and optional parameters for the prompt and GPT model configuration. It returns the predicted class label for the input text based on the examples provided.

#### Example

Suppose you want to classify movie reviews as positive or negative. You can use the following example pairs and input text:

```kotlin
val examples = listOf(
    Pair("I loved the movie. It was amazing!", "Positive"),
    Pair("It's a terrible movie. I wasted my time.", "Negative"),
    Pair("An excellent film with a great story.", "Positive"),
    Pair("The movie was boring and predictable.", "Negative")
)

val inputText = "This movie is a masterpiece and deserves all the praise."
```

You can call the `llm.classify` function with these values:

```kotlin
val result = classify(inputText, examples)
logger.info(result)  // Output: "Positive"
```

The function returns "Positive" as the predicted class label for the input text, indicating that the review is positive.
