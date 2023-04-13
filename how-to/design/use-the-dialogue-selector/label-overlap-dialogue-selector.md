# Label Overlap Dialogue Selector

## Strategy

The Label Overlap Dialogue Selector requires each selectable node to have at least one label assigned. When a dialogue is being selected, its labels are compared with the labels of the dialogue selected previously. The candidate with the most labels matching the previous dialogue is selected. Each dialogue can be selected only once per session.

#### Example

Consider the following situation:

Already triggered dialogue: `Dialogue1("labelA", "labelB", "labelC")`

Dialogue candidates: `Dialogue2("labelA"), Dialogue3("labelB"), Dialogue4("labelC"), Dialogue5("labelA", labelB")`

Given the candidates, Dialogue5 would be selected as it has the largest label overlap with the previous dialogue (labelA and labelB). In the next selection round, the rest of the candidates would be compared with Dialogue5, making Dialogue2 and Dialogue3 equal candidates. In a tie situation, the dialogue is selected randomly.

#### No match

There are situations where none of the candidate dialogues can be selected. This could happen when none of the candidates has a common label with the previous dialogue or if the dialogue is being chosen for the first time in a session (no dialogue has been selected before). In these situations, the dialogue selector acts according to the fallback strategy.

### Fallback strategy

#### Random

If there is no label overlap, the next dialogue is selected randomly from a list of unused selectable nodes.

#### Pool

With this strategy, a non-empty list called _fallback pool_ must be provided (see the Usage section). If there is no label overlap, the next dialogue is selected randomly from the fallback pool.

#### None

If there is no label overlap, the selector returns null and the transition must be specified manually in the selector function.

## Usage

Since the Label Overlap Dialogue Selector has the fallback strategy option and an optional fallback pool, it needs to be instantiated in the init code first.

```
val selector by lazy { LabelOverlapSelector(LabelOverlapSelector.FallbackStrategy.NONE) }
```

and then, it can be used in a function node as follows

```
selectTransition(selector) ?: Transition(fallback)
```

Note that the example above uses the None strategy, meaning that the fallback transition needs to be specified in the selector function. You can select one of the following strategies (as described above): `FallbackStrategy.RANDOM, FallbackStrategy.POOL, FallbackStrategy.NONE.`

If you select the pool strategy, you must provide a list of dialogues that can be used as a fallback. The following example shows a selector with the pool strategy which selects dialogues whose name starts with "recommendation" as a fallback.

```
val selector by lazy { LabelOverlapSelector(LabelOverlapSelector.FallbackStrategy.POOL, prefixedNodeRefs("recommendation")) }
```

Once the selector instance is created, you mark the (Subdialogue) nodes selectable using the R-code tab.

![](<../../../.gitbook/assets/image (15) (1).png>)

Similarly to the Random Dialogue selector, you need to specify the `beforeSelect` block with the `ref` method. The difference is that you need to provide the `ref` method with one or more string parameters representing the node labels.

```
beforeSelect = { 
  ref("movies")
}
```

Additionally, you can create the node reference conditionally, meaning that the node is marked as selectable only under certain conditions. E.g.:

```
beforeSelect = {
    if (someNumber > 5) {
        ref("movies")
    } else {
        null
    }
}
```

The code above means that the corresponding node is selectable (with the label "movies") only if the variable `someNumber` is greater than 5. You can use an arbitrary condition to modify the behavior.

## Advanced

The mentioned fallback strategy is most suitable for situations where the fallback dialogues can estimate labels for the following dialogues. Imagine the following scenario:

> There are several topics the app is able to talk about: movies, sports, books,... At the beginning of the conversation, the app needs to choose a topic to start with. The app has a recommendation dialogue that asks several questions and based on the asnwers it selects the most suitable topic.

In general, the recommendation dialogue needs to add labels for a future decision of the dialogue selector. Several steps need to be followed to achieve this behavior.

1. The subdialogue must accept a parameter of type String with the name of the parent dialogue. In the Properties tab of the dialogue, you need to add `selectorDialogue: String = ""` in the Parameters input field.
2. The subdialogue needs a reference (name) of the parent dialogue where the selector is used. In the Code tab of the corresponding subdialogue node, you need to pass the name as a parameter using the following command: `mapOf("selectorDialogue" to dialogueNameWithoutVersion)`  ![](<../../../.gitbook/assets/image (16) (1).png>)                             &#x20;
3. Define the selector state in the init code of the subdialogue:                                                        &#x20;

```
@Attribute("selectorState")
val parentSelectorState by session(selectorDialogue) { SelectorState() }
```

&#x20;4\.  You can then modify the labels for the selector using the following code

```
parentSelectorState.nodeRefs += LabelOverlapSelector.labelsToNodeRef("movie")
```

The example above adds the label "movie", so during the next selection step, the selector selects a dialogue with this label if available.
