# External API Calls

To call any API endpoint you can use methods get, put, post or delete of object api, e.g.`api.get<DynamicMutableList>(api.target("https://server.com/path/to/someObjects")) `\
`api.post<Order>(api.target("https://server.com/orders"), order, `\
`/* optional headers */ `\
`mapOf( `\
`   "x-header1" to "value1",  `\
`   "x-header2" to "value2"  `\
`  )`\
`)`

where ResponseType is any data class or Dynamic or supported _Type_MutableList (see [https://promethist.ai/apidoc/core-api/com.promethist.core.type/index.html](https://promethist.ai/apidoc/core-api/com.promethist.core.type/index.html))

#### Examples <a href="examples" id="examples"></a>

`api.get<StringMutableList>(api.target("https://repository.promethist.ai/data/animals.json"))`

DialogueScript has several 3rd party API integration preconfigured so you don’t need to specify URLs, headers (e.g. API keys):

### Pre-integrated APIs <a href="pre-integrated-apis" id="pre-integrated-apis"></a>

#### Words API <a href="words-api" id="words-api"></a>

to get list of words use method\
`api.words(word, type)`

to get all results for word (or response of dynamic structure)`api.words<ResponseType>(word, type = "")`

where

* word is input word
* type can be synonyms, antonyms, examples, … (see [https://www.wordsapi.com/docs/#get-word-details](https://www.wordsapi.com/docs/#get-word-details))

**Examples**

Getting list of antonyms\
`api.words("good", "antonyms")`

Processing all available results`api.words<Dynamic>("hatchback")<List<PropertyMap>>("results") { `\
`   value.forEach {  `\
`     println("partOfSpeech = " + it["partOfSpeech"])  `\
`     if (it.containsKey("synonyms")) {  `\
`     println("synonyms:")  `\
`     (it["synonyms"] as List<String>).forEach {  `\
`       println("- $it")  `\
`       }  `\
`     }  `\
`   }  `\
`}`

{% hint style="info" %}
Response caching can be implemented in the future to prevent repeated API requests using the same input data.
{% endhint %}
