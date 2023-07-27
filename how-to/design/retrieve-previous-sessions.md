# Retrieve previous sessions

Function : `history.sessions(limit: Int? = null, from: DateTime? = null, to: DateTime? = null)`

Description: This function retrieves sessions based on optional parameters, such as limit, start time, and end time. When no parameters are provided, all user sessions are returned.

Parameters:

* `limit` (Int, optional): The maximum number of sessions to retrieve.
* `from` (DateTime, optional): The start time to filter sessions. Only sessions after this time will be included.
* `to` (DateTime, optional): The end time to filter sessions. Only sessions before this time will be included.

Returns:

* `Sequence<Session>`: A sequence of Session objects matching the provided parameters.

Example Usage:

{% code overflow="wrap" %}
```kotlin
val sessions = history.sessions(from = java.time.LocalDate.of(2023, 7, 1).atStartOfDay(java.time.ZoneId.of("ECT")), to = java.time.LocalDate.of(2023, 7, 7).atStartOfDay(java.time.ZoneId.of("ECT")))
```
{% endcode %}

{% code overflow="wrap" %}
```kotlin
val sessions = history.sessions(limit = 10)
```
{% endcode %}

Please note that the first session in the list is the current session!

Example of accessing sessions attributes:

{% code overflow="wrap" %}
```kotlin
val clientType = (sessions.toList()[1].attributes["_global"]["clientType"] as Memory<String>).value
```
{% endcode %}

Example of accessing sessions attributes:

{% code overflow="wrap" %}
```kotlin
session.turns.forEach { turn ->
    val nodeId = (turn.attributes["_global"]["nodeId"] as Memory<Int>).value
}   
```
{% endcode %}

TODO:

* Removing the current session from the first position
* Easier access to attributes
* Easier date specification
