---
description: >-
  There is nothing more sad than an empty dialogue model. Let's fill it with
  some content!
---

# Design It

## The graph representation

A [dialogue model](../create-dialogue.md) can be represented as a **graph structure**, which can be viewed and edited in the [Dialogue Designer](../../../app/working-space/design/dialogue-designer.md). The structure consists of different types of **nodes**, which represent the stuff that happens during a conversation, such as speaking, listening, or decision-making. Nodes are interconnected by small arrows or "**transitions**", which define the possible paths of a dialogue. Such a visual structure will help you understand clearly the flow of the dialogue.

{% hint style="info" %}
Before we continue, it must be mentioned that **dialogue models can be interconnected** \(a model can be nested inside another\) and create complex conversational structures. We will talk about this in [another article](../dialogue-linking.md). Here, we describe the designing process inside **one dialogue model**.
{% endhint %}

## Uncover the mystery of nodes

Whenever a new dialogue model is [created](https://docs.promethist.ai/how-to/design/create-dialogue), it will open in the Dialogue Designer. In the main editing area, you will see a default built-in structure: Enter—Speech—Exit. But there are many more types of nodes. Let's see how you can use them to achieve particular designing goals.

![](../../../.gitbook/assets/image%20%2842%29.png)

### Begin the dialogue flow

Any model must always start with an **Enter** node. This is where the bot will _enter_ the dialogue model and begin the conversational adventure. It goes without saying that each dialogue model must have _exactly one_ Enter node, otherwise the bot wouldn't know where to start!

If you want to begin the conversation differently depending on the context, **Enter** will have to be followed by a **Function**, where you will define the decision process, according to which the flow will branch into multiple paths.

### Leave the dialogue flow

TODO

Unlike the Enter node, 

### Make the bot speak

The \(blue\) **Speech** node is one of the most important nodes in your dialogue. Here you can specify what your chatbot will say to the user. Click on the node and write the chatbot speech in the **Texts** field in the right panel \(the red circle in the screenshot below\). – co řádka, to je varianta \(tohle basic, pokud víc  –&gt; odkaz na speak flexibliy\)

![To be replaced by a more representative one](../../../.gitbook/assets/snimek-obrazovky-2021-02-02-v-11.08.57.png)

### Make the bot listen

In order to include the user speech in the conversation, use two following nodes: \(green\) **User Input** node and **Intent** node. \(dva obrázky\)

1. In the left panel **select the "Nodes" tab** \(the red circle in the screenshot below\).
2. Make sure the chatbot will listen while the user is speaking by using the User Input node – **drag and drop it into the graph**.
3. Repeat the same process with the Intent node.
4. Link it to other nodes in the graph by using the **transition** line – hover above the Speech node inside graph and choose one of ports to connect node to another one by dragging **transition**

Předpřipravit na Global Intent

### Exit the dialogue

Just as the **Enter** node, the **Exit** node is necessary to make the dialogue work. At the end of every dialogue, there must be an Exit node whose functionality is not editable in the dialogue designer.

So you have just created the simplest functional dialogue possible but another very important part of the conversation is missing – the user.

### 



