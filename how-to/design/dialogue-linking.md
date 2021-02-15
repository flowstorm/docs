---
description: >-
  If you want to achieve a complex interaction, using only one dialogue model
  would make the dialogue structure very confusing. Let's take a look at how to
  interconnect different dialogue models.
---

# Interconnect Dialogue Models

More interconnected dialogue models ensure:

1. a better orientation in the whole interaction;
2. the possibility to divide the interaction into smaller dialogue models, so that they can be built and created individually by independent [dialogue designers](../collaborate.md);

### Link your main dialogue to other dialogues

Usually, there will always be one main dialogue model that will contain more **subdialogues**. Each subdialogue represents one semantic unit and can contain other subdialogues \(which can also contain other subdialogues etc.\).

Interconnecting the dialogue models is very simple:

1. Insert a Subdialogue node.
2. In the right panel, click on the grey bar called Dialogue.
3. Choose the dialogue model you want to interconnect the main dialogue model with.
4. If you want to open the chosen subdialogue along with the main dialogue model, click on the blue box with a white arrow. The subdialogue opens in a new tab. 

![SEM P&#x158;IJDE GIF&#xCD;K](../../.gitbook/assets/orionthemes-placeholder-image-1.png)

Having interconnected all dialogue models of the interaction, you can create your own [application](../applications/).

### **Inhertited global intents**

Now you have to wonder if the global intents inserted in the main dialogue model work at all levels of the interaction \(= in all interconnected subdialogues\). **The answer is yes.** You don't have to insert the most common global intents in **all** subdialogues, you just have to make sure they are all **above** the subdialogues you want them to be active in. This is something we call **inherited global intents**. All subdialogues inherit the function of the global intents in the main dialogue model. 

{% hint style="info" %}
This article is just a stub. A much more detailed version is coming soon!
{% endhint %}



