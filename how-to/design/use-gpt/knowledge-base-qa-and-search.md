---
description: How to use knowledge base QA and search in the Platform
---

# Knowledge Base QA and Search

### Introduction

In this documentation, we will introduce two primary functions within the Platform: `qa.search` and `qa.answer`. These functions are crucial for accessing the knowledge base and answering your questions. Additionally, we'll provide information about the `KBDocument` class, which is essential for understanding the data structure used by `qa.search`.

### Table of Contents

1. Using `qa.search`
2. Using `qa.answer`
3. Understanding the `KBDocument` Class

***

### 1. Using `qa.search`

The `qa.search` function is used to search for information within the knowledge base. It allows you to retrieve relevant documents that contain potential answers to your questions.

#### How to Use `qa.search`

```kotlin
val results = qa.search(question, numberOfResults)
```

* `question` (String): Your question or query.
* `numberOfResults` (Optional, default=3): The number of relevant documents to retrieve.

#### Returns

* `results` (List of `KBDocument` objects): A list of relevant documents containing information related to the query.

#### Example

Suppose you want to find information about "climate change". You can use `qa.search` like this:

```kotlin
val searchResults = qa.search("What is climate change?")
```

This will return a list of relevant documents that may contain answers to your question.

***

### 2. Using `qa.answer`

The `qa.answer` function is used to generate answers to specific questions based on the retrieved documents from the knowledge base via a large language model.

#### How to Use `qa.answer`

```kotlin
val answer = qa.answer(question, prompt)
```

* `question` (String): Your question or query.
* `prompt` (String): Additional context or information to assist in generating an answer.

#### Returns

* `answer` (String): The generated answer to the question based on the retrieved documents and provided prompt.

#### Example

Suppose you want to generate an answer to the question "What are the causes of climate change?". You can use `qa.answer` like this:

```kotlin
val answer = qa.answer(
    "What are the causes of climate change?",
    "Please answer the question if it is answerable based on the knowledge provided."
)
```

In the first step, `qa.answer` utilizes the `qa.search` function to retrieve relevant documents from the database, in the second step, it feeds the documents together with your question and prompt to a large language model in order to provide an answer. This will generate an answer based on the retrieved documents and the generic prompt provided. The full prompt and logic can be seen below.

**Source Code for `qa.answer` Function:**

```kotlin
fun qa.answer(question: String, prompt: String): String {
    val retrievedDocuments = qa.search(question)
    val combinedPrompt = "$prompt\nKnowledge:\n${retrievedDocuments.joinToString(separator = "\n", prefix = "- ")}\nQuestion: $question\nAnswer:"
    val answer = largeLanguageModel.complete(combinedPrompt)
    return answer
}
```

***

### 3. Understanding the `KBDocument` Class

The `KBDocument` class is a data structure used within the Platform to represent information retrieved from the knowledge base. It contains the following properties:

* `answer` (String): This field is empty in the current knowledge base configuration, as the answer generation is handled by an LLM.
* `score` (Double): A numerical value representing the relevance or confidence level of the document.
* `context` (String): The full retrieved document in string form.
* `documentIds` (List of Strings): Unique identifiers for the document.

#### Example

Here is an example of a `KBDocument` object:

```kotlin
val document = KBDocument(
    answer = "",
    score = 0.85,
    context = "Climate change refers to long-term shifts in temperatures and weather patterns. These shifts may be natural, but since the 1800s, human activities have been the main driver of climate change, primarily due to the burning of fossil fuels (like coal, oil, and gas) which produces heat-trapping gases.",
    documentIds = listOf("d2a0b178c8e6cda845b86b203fd2531f")
)
```

In this example, the `KBDocument` represents information about the causes of climate change. You can access different fields of the `KBDocument` class in Kotlin as follows:

```kotlin
val answerText = document.answer
val relevanceScore = document.score
val additionalContext = document.context
val uniqueIds = document.documentIds
```

These variables will contain the respective values from the `KBDocument` object.
