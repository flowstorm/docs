---
description: >-
  The Dialogue Selector selects a node that comes next (typically a subdialogue
  node) based on the selection strategy and dialogue history. This approach
  allows a more dynamic flow than a fixed transit.
---

# Dynamize the flow (Dialogue Selector)

{% hint style="warning" %}
This article is more technical and requires basic programming knowledge.
{% endhint %}

## Motivation

The dialogue models designed using the Flowstorm platform leverage the tree-like structure of nodes and transitions. This approach allows users to create a conversation flow for various scenarios easily. There are, however, situations where the conversation flow cannot be deterministically predefined during the dialogue creation process. In such situations, the conversation context (and possibly other data) need to be considered to choose the most suitable path in the conversation. Imagine the following situation:

> You start a conversation with a stranger. Such a conversation typically starts with a greeting followed by a "how are you?" question. However, the next steps in the conversation cannot be generally determined as they depend on the external context such as the place you met the stranger, the available information about them, time, situation, weather, etc. Before choosing what to talk about next, you are simply considering multiple facts.

The Dialogue Selector is a tool that considers multiple information sources based on the selected strategy, as in the use case illustrated above.

## Usage <a href="#motivation" id="motivation"></a>

The Dialogue Selector is a (function) node in a dialogue where the next node is selected from a pool of selectable nodes.

### Dialogue Selector Node (Function)

The Dialogue Selector node is a function where the actual selection is processed. Please note that only one selector per dialogue can be defined. It has no outgoing transitions and it is a good idea to choose a friendly name (without spaces) to reference it in the selectable nodes.

![Dialogue Selector node](<../../../.gitbook/assets/image (8) (1).png>)

The most basic code example is to use the following code to call the Random Dialogue Selector.

```
selectTransition(RandomSelector)!!
```

When the conversation reaches the function with the code above, the Random Selector (randomly) selects the next subdialogue from the selectable nodes. Once the selected subdialogue is finished, the conversation flows further using the outgoing transitions from the Subdialogue node. A typical use case is to transit back to the Selector node and to select the next subdialogue.

### Selectable Node

Currently, the only selectable nodes are the Subdialogue nodes. To mark the Subdialogue node as selectable, you need to provide the minimal "R-code".

![R-Code tab](<../../../.gitbook/assets/image (9) (1).png>)

The minimal code is shown below. For advanced usage of the "R-code", see the [Label Overlap Dialogue Selector](broken-reference).

```
beforeSelect = {
  ref()
}
```

The Dialogue Selector considers the Subdialogue nodes with the code above during the selection phase.

As the Subdialogue nodes need to have an outgoing transition, it is a good practice to add a function node that transits back to the Dialogue Selector node using a piece of code. It makes the overall dialogue more readable as it does not require many transition edges to be visible.

![Transition node](<../../../.gitbook/assets/image (10) (1).png>)

### Optional

In the "R-code" tab, you can optionally specify code that is triggered after the subdialogue is selected but before the actual subdialogue is triggered. You can define a logic, e.g., to save particular attributes right after you know which subdialogue was selected. See the following example that prints the ID of the selected node.

```
afterSelect = {
  logger.info("The selected node ID is $id")
}
```
