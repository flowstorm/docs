# Aggregates Attributes

Aggregated attributes can be used to retrieve session and turn attributes from the previous sessions of the user. Additionally, the following methods allow you to retrieve specific information from aggregated attributes, such as the last recorded value, the first recorded value, the most common value, unique values, value counts, maximum value, minimum value, and average value.

## Aggregated Attribute Definition

You can define aggregated attributes similarly to regular attributes using `by turnAggregation` and `by sessionAggregation` delegates. The attribute from the history is selected by the name of the variable.

The following example retrieves the session/turn attribute called "mySessionAttribute/myTurnAttribute" from the previous and current session and returns `AggregatedMemory` object (see below for description):

<pre class="language-kotlin"><code class="lang-kotlin">val mySessionAttribute by sessionAggregation { "" }
<strong>val myTurnAttribute by turnAggregation { "" }
</strong></code></pre>

Suppose you need to use both the regular attribute and the aggregated attribute within the same dialogue. In that case, you must declare the aggregated attribute with a different variable name and annotate it with the actual attribute name as follows:

```kotlin
val myAttribute by session { "" }

@Attribute("myAttribute")
val myAggregatedAttribute by sessionAggregation { "" }
```

Both `sessionAggregation` and `turnAggregation` have the following optional parameters:

* `namespace`: A namespace for the aggregation attribute. If not provided, it defaults to the dialogue name without the version.
* `sessionLimit`: The maximum number of sessions to consider for aggregation.
* `from`: The starting timestamp for the aggregation period (String with `YYYY-MM-DD` pattern).
* `to` : The ending timestamp for the aggregation period (String with `YYYY-MM-DD` pattern).
* `default`:  A lambda function that defines the default value for the aggregation attribute. This is the mandatory parameter that is used to infer the type of the attribute.

The following example retrieves aggregated attributes from the last 10 sessions. Please note that it does not necessarily mean that you get a list of 10 attributes. Not all sessions need to have the attribute set:

```kotlin
val mySessionAttribute(sessionLimit=10) by sessionAggregation { "" }
```

The following examples limit sessions by date:

```kotlin
val mySessionAttribute(from="2023-07-01", to="2023-07-31") by sessionAggregation { "" }
val mySessionAttribute(from="2023-07-01") by sessionAggregation { "" }
val mySessionAttribute(to="2023-07-31") by sessionAggregation { "" }
```

## AggregatedMemory Object

Once you've created an aggregation attribute using either `turnAggregation` or `sessionAggregation`, you receive the `AggregatedMemory` object, and you can perform various operations on it using the following methods:

* `values():` Returns all values from the previous and current sessions
* `last()`: Returns the last recorded value for the aggregation attribute.
* `first()`: Returns the first recorded value for the aggregation attribute.
* `mostCommon()`: Returns the most frequently occurring value in the aggregation attribute.
* `uniqueValues()`: Returns a set of unique values recorded in the aggregation attribute.
* `uniqueCounts()`: Returns a map that counts the occurrences of each unique value in the aggregation attribute.
* `max()`: Returns the maximum value in the aggregation attribute. (Note: Values need to be numerical.)
* `min()`: Returns the minimum value in the aggregation attribute. (Note: Values need to be numerical.)
* `avg()`: Returns the average value of the aggregation attribute. (Note: Values need to be numerical.)

Examples:

<pre class="language-kotlin"><code class="lang-kotlin">// Init code
val mySessionAttribute by sessionAggregation { "" }
<strong>
</strong><strong>// In Function or UserInput node
</strong>mySessionAttribute.values()
mySessionAttribute.first()
mySessionAttribute.uniqueValues()

</code></pre>

## Conclusion

Aggregation attributes, along with the `turnAggregation` and `sessionAggregation` delegate functions, offer a powerful way to manage historical data in your application. By utilizing these features and methods, you can harness the full potential of your data to make informed choices and gain insights into user interactions.
