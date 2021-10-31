# Dialogue Mixins

Dialogue mixins allow conversation developers to **pre-define** unique texts / codes for specific parts of dialogue models and** re-use **them across multiple dialogues within [space](../). Mixin (or trait) id pattern used in several object oriented programming language, allowing developers to include code from multiple different sources into their class.

Following types of mixins are supported:

* `Intent `to define example phrases. Can be used by both contextual and global intents. Use them to pre-define sets of phrases for repeating types of intents (e.g. expressing yes, no, not sure intents)
* `Speech `to define speech variations.
* `Init `to define init code. Useful to declare contextual attributes and/or data properties used by set of dialogues within one or more applications.
* `Function `and `UserInput `to define functional code. It can contain imperative code (e.g. logging or metric counting) and/or lambda function - such lambda can work with set of parameters, including those defining possible transitions and should always express one of such transition - see example below:

![](<../../../.gitbook/assets/image (47).png>)

Lambda should be the last statement of mixin. Function or UserInput using such mixin should contain only calling expression in form (par1, par2, …) - see example using lambda function mixin defined above.

![Intent for positive answer uses example utterances defined in mixin](<../../../.gitbook/assets/image (51).png>)

{% hint style="info" %}
Remember, whenever using mixin lambdas, they must contain (end) with lambda definition of form ({ .. }) and consuming node code must call immediately on the first and the only line in form (par1, par2, …) (otherwise source code composed from dialogue model node code and referenced mixin won’t be valid)
{% endhint %}

Mixin text (code) are always prepended to dialogue model text (code). It is fully on mixin designer to ensure that it is defined valid - it is strongly recommended to create test dialogue model using defined mixins to ensure that model can be built using them.

You can use multiple mixins of specified type in particular context.
