# Snippets

## Main dialogue snippets  <a id="main-dialogue-snippets"></a>

**Cover all necessary global intents and global actions with one drag-and-drop.**

> Every application must have its "main" dialogue, which defines global behavior of the app: What should happen after you launch the app? What should be the generic response to intents like "can you repeat it" or "stop" and to situations like if the user responds something unexpected or doesn't say anything? The main dialogue is the main entrance of the application which can - and often does - redirect the conversation into other subordinate dialogues called subdialogues.

Insert the main dialogue snippet and - just like that - you've got a fully functioning \(although simple\) app structure, which you can now start modifying to your own image.

The snippet is composed of:

* basic global actions \(see below\)
* basic global intents \(see below\)
* the main user input + three generic intents
* various GoBack nodes
* various Sleep nodes

Plan:

* simple \(EN, CS\)
* complex \(more global intents, subdialogues\) \(EN, CS\)

## Subdialogue snippets  <a id="subdialogue-snippets"></a>

**Create a basic subdialogue structure.**

Plan:

* blank subdialogue
* How are you?
* smart random subdialogue
* form filling sequence
* simple quiz \(using data from a file\)

## Functional snippets  <a id="functional-snippets"></a>

**Insert a group of nodes that do something special.**

Plan:

* basic global actions
* basic global intents
  * include suicide etc.
* optional global intents
  * why are you asking
  * I don't wanna tell you
  * I love you
  * I hate you
  * what's your name etc.
* thank you + you're welcome
* different types of questions + intents
  * various entity types detection
  * multiple entities
* userinput ignores head+tail \(using mixin?\)
* first 2 times left, 3rd time right
  * etc.

## Structural snippets  <a id="structural-snippets"></a>

**Accelerate the designing process by inserting pre-set structures.**

Plan:

* analyze existing content and detect most frequent:
  * forks
  * chains
  * co-occurences
* examples: intent+function+speech

