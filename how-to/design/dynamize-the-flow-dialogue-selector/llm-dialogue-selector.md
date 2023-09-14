# LLM Dialogue Selector

The `LLMSelector` class is responsible for selecting the most appropriate subdialogue. It leverages the capabilities of a large language model (LLM) to achieve this objective. Below we detail how the LLM selector operates.

## Overview

At the heart of the `LLMSelector` is a large language model that processes the conversational context along with other relevant information to choose the best path in a dialogue tree. The selector takes into consideration various factors including the current context, dialogue history, user profile, and a set of predefined dialogues, each labeled with a high-level description.

The `LLMSelector` class uses the large language model to generate a prompt that includes the base prompt text, a list of relevant node references from the dialogue library, a transcript of the current dialogue, any additional information provided, and a history of previously selected dialogues. This assembled prompt is then fed into the LLM, which returns a response representing the selected dialogue number.

### Prompt

The LLM Selector is responsible for assembling a structured prompt that will be fed to the large language model for determining the next step in the dialogue. The constructed prompt is created using various pieces of contextual information and has the following structure:

1. **Base Prompt**: This is a predefined string that instructs the LLM on its main objective, which is to facilitate a meaningful, coherent, and engaging conversation.
2. **Dialogue Library**: A list of potential dialogue options each accompanied by a high-level description. These options are available for the LLM to choose from.
3. **Transcript of Dialogue**: A summary of the recent dialogue history, limited to a predefined number of turns, which gives the LLM a view of the ongoing conversation.
4. **Additional Info**: Optional parameter where any extra information can be added to guide the LLM in making a selection.
5. **History of Selections**: This part lists the history of previously selected dialogues to avoid repetitions and to maintain a coherent flow in the conversation.
6. **Selection Instruction**: The final part of the prompt instructs the LLM to output the number of the dialogue to continue the conversation.

You can modify Base Prompt and Additional Info. Moreover, you can change the description of selectable subdialogues. The Transcript of Dialogue, History of Selections and Selection Instruction are created automatically.

#### Example of a Complete Prompt

Below is an illustrative example of how a complete prompt might look:

```plaintext
As the dialogue manager for a digital persona, your objective is to select the optimal dialogue from the point of meaningfulness, coherence, and engagingness. You have access to a library of dialogues numbered and accompanied by a short high-level description. Additionally, you will be provided with a transcript of the conversation, a user profile, and a history of previously selected dialogues. The dialogues must not be repeated, and when the dialogue manager is uncertain about the next selection, select an action that asks the user what they would like to do next or try to learn something new about the user. Your task is to output the number of the dialogue you have chosen to continue the conversation. Remember, your focus is on facilitating a meaningful, coherent, and engaging conversation.

Dialogue Library:
1: Discussing the latest sports events
2: Philosophical conversation about the concept of time
3: Recommendations for summer holiday destinations

Transcript of Dialogue:
User: What do you think about time?
Digital Persona: Time is a fascinating concept, it's a continuous sequence that governs our perception of reality. Would you like to delve deeper into this topic?

Additional Info:

History of Selections:
1, 2

Number of selected dialogue:
```

#### Default Base Prompt

The default base prompt guides the LLM in selecting a dialogue, emphasizing meaningfulness, coherence, and engagingness as the key criteria. It advises against repeating dialogues and encourages the selection of dialogues that foster a more engaging conversation by asking open-ended questions or learning more about the user.

{% code overflow="wrap" %}
```
As the dialogue manager for a digital persona, your objective is to select the optimal dialogue from the point of meaningfulness, coherence and engagingness. You have access to a library of dialogues numbered and accompanied by a short high-level description. Additionally, you will be provided with a transcript of the conversation, a user profile, and a history of previously selected dialogues. The dialogues must not be repeated, and when the dialogue manager is uncertain about the next selection, select an action that asks user what they would like to do next or try to learn something new about user. Your task is to output the number of the dialogue you have chosen to continue the conversation. Remember, your focus is on facilitating a meaningful, coherent and engaging conversation.
```
{% endcode %}

## How to Use LLM Selector

In this example, we will guide you through setup of LLM Selector that can select conversation about latest sports events, philosophical concept of time or summer holiday destinations.

#### 1. Create a dialogue

Create a dialogue according the picture below.

<figure><img src="../../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

#### 2. Write Dialogue Descriptions

Open each subdialogue node and create a description in R-code. Explain briefly what is the role, topic or goal of the dialogue. Prepend the description with `Des:`. LLM selector recognize this text as dialogue description thanks to this prefix.

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

```
beforeSelect = { 
  ref("Desc: Recommendations for summer holiday destinations")
}
```

#### 3. Define LLM Selector in Init Code

Open the Code and paste into it the following code:

```kotlin
val selector by lazy { LLMSelector() }
```

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The `LLMSelector()` takes optional parameter `llmConfig` in which you can specify configuration of large language model that will make the selection of the dialogue. You can for example specify, that you want to use GPT-4:

```kotlin
val selector by lazy { LLMSelector(llmConfig=LLMConfig(model="gpt-4")) }
```

[Read more about LLMConfig](https://docs.flowstorm.ai/how-to/design/use-gpt/complete)



Moreover, LLMSelector() takes optional parameter `basePrompt` through which you can specify the base part of the prompt.

```kotlin
val selector by lazy { LLMSelector(basePrompt="You are a dialogue selector that selects id of only philosophical dialogues.") }
```
{% endhint %}

#### 4. Call LLM Selector in Function

Insert the following code into the upper function of the dialogue:

```kotlin
selectTransition(selector.select(context, this as SelectorModel, relevantNodeRefs))?:Transition(ByeSpeech)
```

The `ByeSpeech` refers to the name of Bye! speech node. This node serves as fallback if the selection fails.

<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}

{% endhint %}

#### 5. Create Transition Back To Dialogue Selector Node

In order to make another selection after the selected dialogue ends, you have to make a transition back to Dialogue Selector Node. Open the bottom function and paste into it the following code:

```kotlin
Transition(DialogueSelector)
```

The `DialogueSelector` refers to the name of Dialogue Selector function from the previous step.

<figure><img src="../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>



###
