---
description: Use GPT to complete prompt
---

# Complete

You can use GPT in Kotlin code using function `llm.complete(prompt: String, stop: List, config: LLMConfig, taskName: String): String` This function acts as a connector to the GPT API and generates text completion based on the provided prompt. The completion will be stopped if any of the stop words are encountered or if the maximum number of tokens is reached. The function takes in a prompt string, a list of stop words, and an instance of the LLMConfig data class that contains various settings for the GPT model.

**Parameters:**

* `prompt`: A string representing the starting prompt for text generation.
* `stop`: A list of strings representing words that, if encountered during text generation, will stop the generation process.
* `config`: An instance of the `LLMConfig` data class that contains various settings for the GPT model.
* `taskName`: A string intended for analytical purposes.

**Return Value:**

The function returns a string representing the generated text completion based on the provided prompt.

**LLMConfig Data Class:**

The `LLMConfig` data class contains the following properties:

* `model`: A string that specifies which model to use. Currently supported models are `gpt-3.5-turbo`(ChatGPT) and `text-davinci-003`(GPT-3). `gpt-3.5-turbo` is the default value.
* `temperature`: A float value representing the sampling temperature for text generation. A higher temperature will result in more diverse and creative text, while a lower temperature will result in more conservative and predictable text.
* `maxTokens`: An integer value representing the maximum number of tokens (words and punctuation) that the generated text completion can have.
* `topP`: An integer value representing the cumulative probability threshold for text generation. A higher value will result in more conservative and predictable text, while a lower value will result in more diverse and creative text.
* `frequencyPenalty`: A float value representing the frequency penalty for text generation. A higher value will result in less repetition of phrases and words, while a lower value will result in more repetition.
* `presencePenalty`: A float value representing the presence penalty for text generation. A higher value will result in more diverse and creative text, while a lower value will result in more conservative and predictable text.

**Example Usage:**

Minimal working example of using GPT in the dialogue is to use `llm.complete` function in speech node with prompt only.

```
${llm.complete("You are chatbot. Ask users how would they like to be called.")}
```

<figure><img src="../../../.gitbook/assets/image (4) (3).png" alt=""><figcaption></figcaption></figure>

More complicated example how to use GPT in code follows:

```kotlin
// Create a new instance of the LLMConfig data class with custom settings
val config = LLMConfig(model = "gpt-3.5-turbo", temperature = 1.0F, maxTokens = 200, topP = 5, frequencyPenalty = 0.5F, presencePenalty = 0.8F)

// Generate text completion based on the provided prompt and stop words using the GPT API
val prompt = "The quick brown fox jumps over the"
val stop = listOf("lazy", "dog", "fence")
val completedText = llm.complete(prompt, stop, config)
```

In this example, the function generates text completion based on the provided prompt ("The quick brown fox jumps over the") and stop words ("lazy", "dog", "fence") using the GPT API with custom settings specified in the `config` instance of the `LLMConfig` data class.
