---
description: >-
  At some point in the interaction, you might care more about the user's
  emotions rather than the exact content. For such situations, Flowstorm offers
  you tools to analyze the so-called "sentiment".
---

# Analyze Sentiment

Imagine you would like to branch the flow based on what memories the user has of their hometown. More specifically, you don't care about what the memories are precisely about. You would just like to continue the "positive" flow if they have positive memories, the "sad" flow if they have negative memories, or the "neutral" flow if they don't have any particularly strong negative nor positive memories. For this purpose, you can use **sentiment analysis**.

Sentiment analysis classifies text into three categories: **positive**, **neutral**, and **negative**. We can use those categories to branch the dialogue flow.

{% hint style="success" %}
_"I loved the city in which I grew up"_ and _"I like you"_ have a **positive sentiment**.

_"Prague is the capital of the Czech Republic"_ and _"I do sports"_ have a **neutral sentiment**.

_"I hated the street I lived on"_ and _"I don't enjoy loud music"_ have a **negative sentiment**.
{% endhint %}

## Sentiment snippet <a id="sentiment-snippet"></a>

The easiest way to employ sentiment analysis is by using the predefined **Sentiment** [snippet](../../model/dialogue-model-coding/building-blocks/snippets.md). Drag-and-drop it from the snippet palette into your dialogue.

![Using a predefined &quot;dialogue snippet&quot; for sentiment analysis. ](https://gblobscdn.gitbook.com/assets%2F-MUs26EFFf_IPxqoQh7r%2F-MUtDdbdhGN8LU4cXQWo%2F-MUtNeNW78y7FzFTElRW%2Fsentiment.gif?alt=media&token=a4476432-b396-4880-bbc1-45c79a4beb5e)

## Code in the user input node <a id="code-in-the-user-input-node"></a>

If you want to work with the source code to customize the decision, you can proceed from the following code \(in the _UserInput_ node\).

{% hint style="warning" %}
Don't forget to **modify the names of the transitions** according to the transitions in your model.
{% endhint %}

```kotlin
// process input
processPipeline()

// user said something positive
if (input.sentiment.name == Sentiment.POSITIVE.toString()){
    toPOSITIVE_sentiment // MODIFY THE TRANSITION NAME
}

// user said something neutral
else if (input.sentiment.name == Sentiment.NEUTRAL.toString()){
    toNEUTRAL_sentiment // MODIFY THE TRANSITION NAME
} 

// user said something positive
else if (input.sentiment.name == Sentiment.NEGATIVE.toString()){
    toNEGATIVE_sentiment // MODIFY THE TRANSITION NAME
}

// something went wrong, no sentiment detected -> default to neutral sentiment
else {
    toNEUTRAL_sentiment // MODIFY THE TRANSITION NAME
}
```

