# External API Calls

To call any API endpoint you can use methods get, put, post or delete of object api, e.g.`1api.get<DynamicMutableList>(api.target("https://server.com/path/to/someObjects")) 2 3api.post<Order>(api.target("https://server.com/orders"), order, 4 /* optional headers */ 5 mapOf( 6 "x-header1" to "value1", 7 "x-header2" to "value2" 8 ) 9)`

where ResponseType is any data class or Dynamic or supported _Type_MutableList \(see [https://promethist.ai/apidoc/core-api/com.promethist.core.type/index.html](https://promethist.ai/apidoc/core-api/com.promethist.core.type/index.html)\)

#### Examples <a id="Examples"></a>

`1api.get<StringMutableList>(api.target("https://repository.promethist.ai/data/animals.json"))`

DialogueScript has several 3rd party API integration preconfigured so you don’t need to specify URLs, headers \(e.g. API keys\):

### Pre-integrated APIs <a id="Pre-integrated-APIs"></a>

#### Words API <a id="Words-API"></a>

to get list of words use method`1api.words(word, type)`

to get all results for word \(or response of dynamic structure\)`1api.words<ResponseType>(word, type = "")`

where

* word is input word
* type can be synonyms, antonyms, examples, … \(see [https://www.wordsapi.com/docs/\#get-word-details](https://www.wordsapi.com/docs/#get-word-details)\)

**Examples**

Getting list of antonyms`1 api.words("good", "antonyms")`

Processing all available results`1api.words<Dynamic>("hatchback")<List<PropertyMap>>("results") { 2 value.forEach { 3 println("partOfSpeech = " + it["partOfSpeech"]) 4 if (it.containsKey("synonyms")) { 5 println("synonyms:") 6 (it["synonyms"] as List<String>).forEach { 7 println("- $it") 8 } 9 } 10 } 11}`

{% hint style="info" %}
Response caching can be implemented in the future to prevent repeated API requests using the same input data.
{% endhint %}

