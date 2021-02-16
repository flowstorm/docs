---
description: >-
  If you want to design a complex interaction, one large graph structure could
  be very confusing. It's better to design smaller models and interconnect them.
  Let's take a look at how to do it.
---

# Interconnect Dialogue Models

{% hint style="info" %}
This article is just a stub. A much more detailed version is coming soon!
{% endhint %}

One large graph structure can be very confusing. It's better to design smaller models and interconnect them. This way, interconnecting dialogue models ensures:

* a better orientation in the whole interaction;
* the possibility to divide the interaction into smaller dialogue models, so that they can be edited individually by independent [conversation designers](../collaborate.md).

## Link a dialogue to another dialogue

Usually, there will always be one main dialogue model that will contain more **subdialogues**. Each subdialogue represents one semantic unit and can contain other subdialogues \(which can also contain other subdialogues etc.\).

Interconnecting the dialogue models is very simple:

1. Insert a _**Subdialogue**_ node.
2. In the right panel, click on the grey bar called Dialogue.
3. Choose the dialogue model you want to connect to the original model.
4. If you want to open the chosen subdialogue in a new tab, click on the blue box with a white arrow.

Having interconnected all dialogue models of the interaction, you can create your own [application](../applications/).

## **Inhertited global intents**

Now you must be wondering if the global intents inserted in the main dialogue model work at all levels of the interaction \(= in all interconnected subdialogues\). **The answer is yes.** You don't have to insert the most common global intents into **all** subdialogues, you just have to make sure they are all **above** the subdialogues you want them to be active in. This is something we call **inherited global intents**. All subdialogues inherit the global intents from the higher-level models.



