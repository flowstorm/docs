---
description: >-
  Save information into memory, and use it anytime you need. Make personas work
  with context and remember details about the user.
---

# Use variables (memory & context)

{% hint style="warning" %}
This article is more technical – basic programming knowledge is recommended, but we tried to explain the principles as clearly as possible :relaxed:
{% endhint %}

{% hint style="info" %}
In Flowstorm, **variables** are called **contextual attributes** (or just _attributes_).
{% endhint %}

Learning about contextual attributes is important for everyone who wants to use Flowstorm to design more complex conversations. These attributes provide your digital personas with **MEMORY** so that they can remember and recall information, and thus react appropriately.

Contextual attributes are usually declared in the **CODE tab** in the dialogue editor:

![](<../../../.gitbook/assets/image (83).png>)

Alternatively, if you want to declare an attribute for multiple dialogues, use an **INIT MIXIN** ([how to do it?](../define-dialogue-properties.md)).

## The syntax

To declare a contextual attribute, always use the following syntax:

{% hint style="success" %}
**`var/val attributeName   by scope   ("namespace")   { initialValue }`**
{% endhint %}

As you can see, there are 4–5 pieces of information needed:

* the **keyword** of the attribute (`var/val`),
* the **name** of the attribute,
* the **scope** of the attribute (`by session / by user / ...`),
* (_optional_) the **namespace** where the attribute should be stored,
* the **initial value** of the attribute.

Let's have a look at them one by one.

### Keyword

There are two different keywords to declare an attribute: **var** or **val**.

* Use **var** for a variable whose value _**can change**_ (that's the most common case).
* Use **val** for a variable whose value _**never changes**_ (such as a finite list of options).

Example:&#x20;

```
var counter = 5    // Int type is inferred
val PI = 3.14
val c: Int         // Type required when no initializer is provided
```

Attributes are identical to the [Kotlin data types.](https://www.javatpoint.com/kotlin-data-type)

### Name

The name of the attribute:

* cannot start with a number,
* cannot contain blank spaces or special characters (except for the underscore character `_`),
* cannot be identical to the name of an implicit attribute (if it is the case, you will be alerted during the dialogue [build](../../build-and-test.md)).

### Scope

A scope is a region of code that constitutes a context in which attributes and their names can be introduced.&#x20;

Flowstorm has the following context scopes:

* **Session** (`by session`) – remember the value until the end of the session, then forget
* **User** (`by user`) – remember the value for the particular user, don't forget
* **Turn** (`by turn`) _–_ remember the value only during the current turn, then forget
* **Community** (`by comunity`) – save the values from multiple users to one list of values
  * here, the variable's namespace must be filled in and it equals the name of the "user community"
  * the data from community attributes can be viewed in[ Data — Community attributes](https://docs.flowstorm.ai/studio/main-menu/data#community-attributes)
* **Client** (`by client`) – expect the values from the client device _(advanced)_
* **Loader** (`by loader`) – load the data from an external data source _(advanced)_

{% hint style="warning" %}
In this article, we explain in detail only the `session` and `user` scopes. For detailed information about the others, check out the technical documentation or contact our technical support.
{% endhint %}

#### THE SESSION SCOPE (_`by session`_)

`var currentPoints by session { 0 }`&#x20;

This code defines a reassignable variable of the type `Int` (integer) with the initial value `0` in the scope of the session. Starting a new session will reset the attribute to the initial values.

You can use these attributes e.g. to count points in a game. Whenever the user gives the right answer, you can increment the value of the variable using this simple code in a Function node:

`currentPoints++` (add 1)

When a new session starts, the variable is automatically reinitialized, and points are counted again from zero.&#x20;

#### THE USER SCOPE (_`by user`_)

To remember the value of an attribute across multiple sessions, delegate the attribute `by user`:

`var sessionCounter  by user  { 0 }`&#x20;

`var userName        by user  { "" }`

This is an **extremely useful way of personalizing your content**. The first example above might be good for counting how many times a user has opened your application (at the beginning of the dialogue flow, there would have to be a Function with the code `sessionCounter++`). And the second example attribute could store the name of the user.

### Namespace

The namespace is an **optional** parameter (except for attributes `by community`, see above). A namespace helps to group together attributes from different dialogues (similar to the [init mixin](../define-dialogue-properties.md)). An attribute declared without an explicit namespace is accessible only in the dialogue where it is defined, except for attributes declared in init mixins.

### Initial value

Attributes can be of many types, e.g. String, Int, List, DateTime... The type is automatically inferred from the initial value.

Here are some examples of defining attributes:&#x20;

```
var lastSessionTimeStamp   by user                  { now }
val appOpeningsTimeStamps  by user                  { DateTimeMutableList() }
var currentResponse        by session  ("games")    { "" }
val allResponses           by user     ("games")    { StringMutableList() }
```

You can even create your own data classes and declare attributes as instances of these classes.

Here is a definition of a class Pet:

```
data class Pet (var name: String = "", var species: String = "", var age: Int = 0)
    var pet   by user  ("general")  { Pet() }
    val pets  by user  ("general")  { mutableListOf<Pet>() }
```
