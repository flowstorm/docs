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



#### Method Overview

* `select(context: Context, model: SelectorModel, relevantNodeRefs: List<NodeRef>, additionalInfo: String = ""): NodeRef?`: This method leverages the LLM to select an appropriate dialogue node based on the constructed prompt and the given context, model, and list of relevant node references. It returns the selected node reference or null if no appropriate node was found.

###
