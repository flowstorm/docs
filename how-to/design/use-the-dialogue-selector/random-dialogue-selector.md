# Random Dialogue Selector

## Strategy

The Random Dialogue Selector is a basic implementation of the Dialogue Selector. Its strategy is to randomly select the next node from a pool of selectable nodes. When a node is selected, its trigger count is increased. The Random Dialogue Selector always selects the node with the lowest trigger count.&#x20;

Note that generally, depending on the conditions, the Dialogue Selector does not have to always select a node. However, the random strategy always selects a node because a node can be selected repeatedly if all the other nodes have already been selected. This is the reason why you do not need to specify the fallback transition.

## Usage

An example of the Random Dialogue Selector usage:

```
selectTransition(RandomSelector)!!
```

Since RandomSelector is the default option, you can just use the following code:

```
selectTransition()!!
```
