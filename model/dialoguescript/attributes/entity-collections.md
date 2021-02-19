# Entity Collections

### Entity collections in attributes <a id="Entity-collections-in-attributes"></a>

DialogueScript supports working with data entity collections \(lists and sets\) as attributes, storing automatically their names as references into attribute values \(due to technical reasons, data entities cannot be stored directly\). Data class must implement **NamedEntity** interface to provide **name** property used by entity collection attribute delegate as instance reference.

Scopes turn, session and user are supported \(community not yet\). Following delegate functions are provided, requiring data entity collection as first and optional second namespace parameters

* **turnEntityListAttribute**, **sessionEntityListAttribute** and **userEntityListAttribute**
* **turnEntitySetAttribute**, **sessionEntitySetAttribute** and **userEntitySetAttribute**

#### Example <a id="Example"></a>

```kotlin
// init code
data class Movie(override val name: String, val director: String, val year: Int) : NamedEntity

val movies by loader<List<Movie>>("./resources/movies")
val likedMovies by userEntityListAttribute(movies)

// functional code
val movie = movies.similarTo(input, { name }, 0.5)
if (movie != null)
  likedMovies.add()

// response
You like #{likedMovies.list { name }, subj = "movie"}.
```

{% hint style="warning" %}
Name should be considered as primary key thus unique and not changing in time. If your data class define different data suitable to be used as unique name, you can provide it via getter, see following example.`1`
{% endhint %}

```kotlin
data class Person(val givenName: String, val surname: String, val birthPlace: String) : NamedEntity {
  override val name get() = "$givenName $surname born in $birthPlace"
}
```

### Maps in attributes <a id="Maps-in-attributes"></a>

You can use attributes to store maps mapping String keys to values of Any type. Following delegate functions are available to do this

* **turnMapAttribute**, **sessionMapAttribute** and **userMapAttribute**

#### Example <a id="Example.1"></a>

```kotlin
// init code
val test by sessionMapAttribute(mapOf("a" to "first", "b" to "second", "c" to "third"))

// functional code
test.put("a")
test.put("c")

logger.log(test.toString()) // this will return mapOf("a" to "first", "c" to "third")
```

