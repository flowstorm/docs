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

### Balancing the Number of Examples for Cost and Accuracy

When using the `llm.classify` function for text classification, it's important to balance the number of examples per class to achieve reasonable classification accuracy while keeping the costs under control. This section will provide some insights into this aspect of the function.

#### Adequate Number of Examples

The GPT model can achieve reasonable classification accuracy with just a few examples per class. In most cases, providing 2-5 examples per class should be sufficient for the model to grasp the general pattern and perform well on unseen input texts. However, the optimal number of examples may vary depending on the complexity of the classification task and the similarity between classes.

#### Longer Prompts and Increased Cost

Adding more examples to the classification task will result in a longer prompt for the GPT model. Since the cost of using GPT models is often determined by the number of tokens processed (which includes both input and output tokens), a longer prompt will result in higher costs.

To strike a balance between classification accuracy and cost, it is recommended to:

1. Start with a small number of examples per class (e.g., 2-5) and evaluate the classification performance on a validation set.
2. If the performance is not satisfactory, you can incrementally add more examples and re-evaluate the performance.

By following these guidelines, you can achieve acceptable classification accuracy using the `llm.classify` function without incurring excessive costs.
