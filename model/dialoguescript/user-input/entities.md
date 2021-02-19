# Entities

You can access recognize entities form the user utterance in the input object. The entities are recognized using multiple models. The list of models may \(and it will\) change in the future:

* SpaCy small model
* Alquist NER neural network
* Duckling service

To get an entity, you need to know a type of entity you want to work with. The supported entity type are:



| TYPE | DESCRIPTION |
| :--- | :--- |
| PERSON | People, including fictional. |
| NORP | Nationalities or religious or political groups. |
| FAC | Buildings, airports, highways, bridges, etc. |
| ORG | Companies, agencies, institutions, etc. |
| GPE | Countries, cities, states. |
| LOC | Non-GPE locations, mountain ranges, bodies of water. |
| PRODUCT | Objects, vehicles, foods, etc. \(Not services.\) |
| EVENT | Named hurricanes, battles, wars, sports events, etc. |
| WORK\_OF\_ART | Titles of books, songs, etc. |
| LAW | Named documents made into laws. |
| LANGUAGE | Any named language. |
| DATE | Absolute or relative dates or periods. |
| TIME | Times smaller than a day. |
| PERCENT | Percentage, including ”%“. |
| MONEY | Monetary values, including unit. |
| QUANTITY | Measurements, as of weight or distance. |
| ORDINAL | “first”, “second”, etc. |
| CARDINAL | Numerals that do not fall under another type. |

In addition to these types, there are several types which also contains the structured information about the entity \(see Duckling section of this page\).

Here is an example of how to get the list of entities with type “EVENT”.

```text
val events = input.entities("EVENT")
```

Equivalently, you can get the first entity of the given type directly

```text
val event = input.entity("EVENT")
```

Each entity has the following fields you can work with

```text
event.className   // Entity class, EVENT in this example
event.text        // The text from the original utterance representing the entity
event.confidence  // Confidence returned by the model 
                  // (0.5 constant for SpaCy model and 1.0 for Duckling model)
event.modelId     // The ID of the model that recognized the entity
```

If you need to work with all entities regardless of the type, you can get the full list using the following command.

```text
val entities = input.entityMap.values.flatten()
```

You can obtain the first entity directly using input.entity\("PERSON"\) \(WARNING: throws an exception if the is no entity of that type, you need to check it first using input.containsEntity\("PERSON"\)\)

The alternative is to obtain a full list using input.entities\("PERSON"\)

## Required Entity in the Intent Node <a id="Required-Entity-in-the-Intent-Node"></a>

The most common use-case is when you need an Intent node which also requires an entity. Assume this example situation:

You have a UserInput node with two intents

* My favorite book is The Godfather
* I don’t have a favorite book

The first intent node also needs the entity of type book to be recognized, otherwise, the following conversation wouldn’t make sense. You can specify the required entity types for the given intent node in the properties of the node in the field “Entities” \(multiple entity types are comma-separated, no space!\)

![](../../../.gitbook/assets/image%20%2859%29.png)



