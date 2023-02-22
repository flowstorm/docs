# \*Design complex logic using code

More complex logic needs to be designed in code. Code is typically written in the _**init code**_ of the dialogue model, in _**Function**_ nodes, _**User Input**_ nodes, and _**Speech**_ nodes.

{% hint style="success" %}
In Flowstorm, use the **KOTLIN** programming language: [https://kotlinlang.org/](https://kotlinlang.org/)
{% endhint %}

{% hint style="info" %}
In most cases described below, you will need to know _**how to declare a variable**_. Read about it in [**this article**](work-with-variables.md)!
{% endhint %}

## Init code of the dialogue model

The init code is executed every time the dialogue model is launched. It is an integral part of the model, and typically contains declarations of **variables** (more about that [here](work-with-variables.md)).

The init code is defined in the **Code** tab of a dialogue model:

![](<../../../.gitbook/assets/image (84).png>)

{% hint style="info" %}
If you want to reuse the same init code in multiple dialogues, use the **init mixin**. To learn how to do it, check out [this article](../define-more-properties.md).
{% endhint %}

## Dialogue logic in Functions & User Inputs

The Function and User Input nodes allow you, among other things, to:

* read and rewrite values of variables,
* branch the dialogue flow based on a condition,
* call external APIs,
* the code in _User Inputs_ also allows you to manually pre-process the user message.

### Simple conditions in Functions

Here's an example of a simple if-then-else structure in a Function node (note: the variables `condition1` and `condition2` would have to be [defined](work-with-variables.md) in the init code, otherwise it would cause a build error):

![](<../../../.gitbook/assets/image (88).png>)

Functions typically **return transitions.** In other words, the "result" of the function is always the **name of the transition where the flow should continue**.

For example, in the following image, the three transitions leading from the Function node are called `toSpeech1`, `toSpeech2`, and `toSpeech3`. You can view the name if you hover over the arrow, as seen in the image; clicking on the arrow will show the transition detail in the right panel, where you can also rename it (however, _**the name should always follow the format**_ `toX`).

![](<../../../.gitbook/assets/image (87).png>)

{% hint style="warning" %}
If there is **no transition** at the end of a function, the digital persona will **randomly** choose one of the connected transitions.

This can be helpful if you want to branch the flow randomly – just insert an empty Function and connect multiple transitions.
{% endhint %}

### Processing the input in User Inputs

In User Inputs, you can use the same type of code as in Functions. But in most cases, you will need a transition to **intent recognition**. That's what the pseudo-transition "**`pass`**" does.

In the following simple example, the digital persona will first analyze the input text to see if it contains the string "dog". If yes, the flow continues `toSpeech5`; if not, intent recognition is activated and the flow will continue to the most semantically similar intent:

![](<../../../.gitbook/assets/image (92).png>)

## Rules and variables in Speech nodes

You can also write code in Speech nodes if you want the digital persona to:

* read the value of a variable,
* or insert a simple condition without the need for a Function node.

Code in Speeches is written **inside `${}`**, for example:

* _**`Hello, ${username}.`**_
  * The persona will read the string value saved in the variable `username` (needs to be [declared](work-with-variables.md)).
* _**`Your favorite number is ${favoriteNumber}.`**_
  * The persona will read the integer value saved in the variable `favoriteNumber` (needs to be [declared](work-with-variables.md)).
* _**`The game is over. ${if (points > 10) "What a great game!" else if (points > 5) "That was a good game." else "Well, thanks for playing."}`**_
  * If the value of `points` (needs to be [declared](work-with-variables.md)) is greater than 10, the first string will be uttered, else if the value is at least greater than 5, the second string will be uttered, otherwise the third string will be uttered.
