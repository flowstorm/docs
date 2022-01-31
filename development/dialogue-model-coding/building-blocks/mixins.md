---
description: Create a text or function once, re-use them for several times in many places.
---

# Mixins

Dialogue mixins allow conversation developers to **pre-define** unique texts / codes for specific parts of dialogue models and **re-use** them across multiple dialogues within organization (in near future, also share them across multiple organizations). Mixin (or trait) id pattern used in several object oriented programming language, allowing developers to include code from multiple different sources into their class.

Following types of mixins are currently supported:

* _**Intent**_ to define example phrases. Can be used by both contextual and global intents. Use them to pre-define sets of phrases for repeating types of intents (e.g. expressing yes, no, not sure intents)
* _**Speech**_** ** to define speech variations.
* _**Init**_ to define init code. Useful to declare contextual attributes and/or data properties used by set of dialogues within one or more applications.
* _**Function**_ and _**UserInput**_ to define functional code. Can contain imperative code (e.g. logging or metric counting) and/or lambda function - such lambda can work with set of parameters, including those defining possible transitions and should always express one of such transition - see example below:

![](<../../../.gitbook/assets/image (40).png>)

Lambda should be the last statement of mixin. Function or UserInput using such mixin should contain only calling expression in form (par1, par2, …) - see example using lambda function mixin defined above:

![](<../../../.gitbook/assets/image (39).png>)

_**Remember, whenever using mixin lambdas, they must contain (end) with lambda definition of form ({ .. }) and consuming node code must call immediately on the first and the only line in form (par1, par2, …) (otherwise source code composed from dialogue model node code and referenced mixin won’t be valid)**_

Mixin text (code) are always prepend to dialogue model text (code). It is fully on mixin designer to ensure that it is defined valid - it is strongly recommended to create test dialogue model using defined mixins to ensure that model can be built using them.

You can use multiple mixins of specified type in particular context.
