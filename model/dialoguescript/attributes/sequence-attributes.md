# Sequence Attributes

Sequences are special attributes allowing do provide sequence of items of type `String` or named data entities \(objects of data class implementing `NamedEntity` interface\). This is useful if you want give user any kind of questions or tasks and prevent to repeat them \(or pass too often\) by remembering them. Implementation is based on `MemoryMutableSet<String>` storing each value \(data entity name\) into `value` property together with `count` and `time` properties. Developer specifies how next item will be selected implementing lambda `nextBlock` function called by sequence in scope of SequenceAttribute object, providing **properties**

* `next` returns next value generated from sequence. If no more suitable value is available in sequence at the moment, it will be `null`.
* `last` returns last generated value. If there is no access to `next` property yet, it throws error.

and two **methods**

*  `nextRandom`
*  `nextInLine` \(if end of list is reached and `maxCount` value described below is &gt; 1, it will start from the beginning of list again after `minDuration`\)

both with following optional **parameters**

* `minDuration` - minimum duration before repeating same item again \(default is 1.day\)
* `maxCount` - maximum count \(default value `Int.MAX_VALUE` means unlimited\)
* `resetDuration` - if `count` has reached `maxCount` and `time < now - resetDuration` then `count` will be reset to `0` and therefore value can be returned again \(default value `null` means no reset time\). e.g. `1.week`

{% hint style="info" %}
Please remember that `*Duration` parameters work always only within attribute scope - therefore using `hour`, `day` or `week` units make sense to use only in user, not session context [scope](../../dialogue-model-coding/context-scopes/) \(which usually takes only minutes\)
{% endhint %}

{% hint style="warning" %}
Sequence attributes should be always declared as **`val`** because their value is object of type `SequenceAttribute`
{% endhint %}

## Examples

### Init Code

```kotlin
// sequence based on String list, returning items in line, each only once in a minute
val colorSequence by sessionSequenceAttribute(listOf("blue", "green", "yellow")) { nextInLine(1.minute) } 

// sequence based on entity list, returning items randomly, each only once in an hour, and maximum 3 times in a day
val movieSequence by userSequenceAttribute(movies) { nextRandom(1.hour, 3, 1.day) } 

// sequence based on entity list, returning items randomly
data class Example(override val name: String, val result: Int) : NamedEntity
val examples = mutableListOf<Example>().apply {
  (1..10).forEach { a ->
    (1..10).forEach { b ->
      add(Example("$a plus $b", a + b))
      if (b > a)
        add(Example("$a minus $b", a - b))
      if (a * b < 40)
        add(Example("$a times $b", a * b))
    }
  }
}
val exampleSequence by sessionSequenceAttribute(examples) // no nextBlock = nextRandom() will be used by default
```

### Functional code

```kotlin
// next example response
if (exampleSequence.next == null) {
  toNoMoreExamples
} else {
  addResponseItem("What is ${exampleSequence.last.name}?")
  toExampleInput
}

// comparing result to user input
if (exampleSequence.last.result == input.transcript.text.toInt()) 
  toExampleSuccess
else
  toExampleFailure

```

