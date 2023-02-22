---
description: >-
  Create and modify dialogue mixins. These are referenceable contents of
  different types of nodes (Intent, Speech, Command, Function, User Input, or
  the "Init" code of a dialogue).
---

# Mixins (editor)

The following types of mixins are supported:

* `Intent` – can be referenced both by intents and global intents
* `Speech`
* `Init` can be referenced in the init code; it can be useful e.g. for declaring contextual attributes and/or data properties used in multiple dialogues
* `Function` and `UserInput` define functional code. It can contain imperative code (e.g. logging or metric counting) and/or a lambda function – such lambda can work with a set of parameters, including those defining possible transitions, and should always express one of such transitions – see the example below:

![](<../../../.gitbook/assets/image (47).png>)

The lambda should be the last statement of a functional mixin. The Function or UserInput node using such mixin should contain only calling expression in form (par1, par2, …) - see the example using lambda function mixin defined above.

![Intent for positive answer uses example utterances defined in mixin](<../../../.gitbook/assets/image (51).png>)

{% hint style="danger" %}
Remember, whenever using mixin lambdas, they must contain (end) with lambda definition of the form `({ .. })` and consuming node code must call immediately on the first and the only line in the form `(par1, par2, …)` (otherwise, source code composed from dialogue model node code and referenced mixin won’t be valid).
{% endhint %}

Mixin text (code) is always prepended to dialogue model text (code). It is fully the responsibility of the mixin designer to ensure that the mixin is defined as valid. It is strongly recommended to create a test dialogue model using defined mixins to ensure that the model can be built using them.

You can use multiple mixins of the specified type in a particular context.
