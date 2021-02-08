---
description: What you need to know beforehand.
---

# Introduction

## The graph representation

A [dialogue model](create-dialogue.md) can be represented as a **graph structure**, which can be viewed and edited in the [Dialogue Designer](../../app/space/design/dialogue-designer.md). The structure consists of different types of **nodes**, which represent the stuff that happens during a conversation, such as speaking, listening, or decision-making. Nodes are interconnected by small arrows or "**transitions**", which define the possible paths of a dialogue. Such a visual structure will help you understand clearly the flow of the dialogue.

{% hint style="info" %}
Before we continue, it must be mentioned that **dialogue models can be interconnected** \(a model can be nested in another model\) and create complex conversational structures. We will talk about this in [another article](dialogue-linking.md). Here, we will describe the designing process within **one dialogue model**.
{% endhint %}

## Work in the Dialogue Designer – basics

{% hint style="info" %}
These are just the basics. If you want to know more about working with the Dialogue Designer, read [this article](../../app/space/design/dialogue-designer.md).
{% endhint %}

### Drag and drop & connect nodes

To insert an item \(usually a node\) into the structure, just drag it from the left panel and drop it into the canvas. You can then connect the nodes using transitions.

![Drag, drop, connect &#x2013; it&apos;s super easy!](../../.gitbook/assets/drag-and-drop.gif)

### Move, remove, copy & paste nodes

To **MOVE** a node, just drag it along the canvas.  
To **REMOVE** a node, click on it and then press Delete or Backspace on your keyboard.  
To **COPY** a node, select it and press Ctrl+C or click the Copy button.  
To **PASTE** from the clipboard, press Ctrl+V or click the Paste button.

To **select more than one node** and move/remove/copy all of them together, you can:

* either **select a part of the canvas**: click in the graph and wait for half a second, then drag your cursor elsewhere -&gt; the canvas in between will be framed in pink and after releasing the cursor, all nodes inside that area will become selected;
* or **hold Ctrl and select nodes by clicking on one after another**.

Once a group of nodes is selected, you can perform the desired action on all of them.

![](../../.gitbook/assets/group.gif)

