# Custom Dialogue Selector

You can define custom Dialogue Selector logic. You need to extend the Selector interface and implement the `select` method.

```
class MyCustomSelector : Selector {
    override fun select(model: SelectorModel, relevantNodeRefs: List<NodeRef>): NodeRef? {
         // Code here
    }
}
```
