---
description: Use GPT to chat
---

# Chat

The `llm.chat()` function is a text-based conversational interface that uses GPT (Generative Pre-trained Transformer) to generate responses based on the context of a conversation. This documentation provides details on how to use the `llm.chat()` function to generate responses.

## Function Signature

```kotlin
fun chat(
    context: Context,
    numTurns: Int = 5,
    prompt: String = "The following is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly.",
    personaName: String = "System",
    config: LLMConfig = LLMConfig()
): String
```

### Parameters

The `chat()` function accepts the following parameters:

* `context`: The current context of the conversation, represented as a `Context` object.
* `numTurns`: The number of previous turns to consider when generating the response. Defaults to `5`.
* `prompt`: The prompt text to display before the generated response. Defaults to `"The following is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly."`.
* `personaName`: The name of the persona or system generating the response. Defaults to `"System"`.
* `config`: A configuration object of type `LLMConfig`. This parameter is optional and defaults to the [default configuration values](https://docs.flowstorm.ai/how-to/design/use-gpt/complete).

### Return Value

The `chat()` function returns a string representing the generated response based on the input context.

## Example Usage

Here's an example of how to use the `llm.chat()` function:

```kotlin
val response = llm.chat(
    context, 
    numTurns = 5, 
    prompt = "You are digital persona Kai", 
    personaName = "Kai")
logger.info(response)
```

In this example, we pass context, number of turns, prompt and persona's name to the `llm.chat()` function, which generates a response based on the input context. Finally, we print the generated response.
