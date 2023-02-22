---
description: >-
  How to access trivia and fun facts that are scraped from r/todayilearned and
  saved in the Elasticsearch database.
---

# Retrieve and Implement Fun Facts and Trivia

The Flowstorm platform automatically looks for and gathers interesting trivia shared on the r/todayilearned subreddit. The trivia is then saved in the Elasticsearch database.

Interaction designers are able to query the Elasticsearch database to retrieve the trivia and use it in dialogues. The Reddit connector implements the getTrivia() function:

```
reddit.getTrivia() //returns a list of random trivia objects from the database
reddit.getTrivia("food") //returns a list of top scoring trivia objects based on the query string


reddit.getTrivia("food").map{it.title} //retuns a list of strings of top scoring trivia based on the query string
```

The trivia class has the following attributes:

```
data class Trivia(
    val title: String,
    val source: String = "Unknown",
    val score: Int = 0,
    val permalink: String = "",
    val created: Instant = Clock.System.now(),
    val fullname: String = "",
    override val _id: Id<Trivia> = newId(),
) : Entity
```

