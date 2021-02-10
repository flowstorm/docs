# Dialogue Model

## 

## Node Types

### Speech  <a id="speech"></a>

The text in the node will be sent to the Text To Speech converting the text to voice. 

A node can be set as repeatable. If true, the text in this node will be included when the bots repeat a response. See [Repeating responses](https://promethist.myjetbrains.com/youtrack/articles/APP-A-9/Repeating-responses) for the detailed explanation.

### Intent  <a id="intent"></a>

Intent nodes serve to represent the user’s **utterances**. These are similar to the intent node but they do not start a branch, they continue one. Usually, several nodes are connected to an input node. Several Contextual Intent nodes are provided with pre-filled common utterances. These nodes have a “Negate“ function \(available above the text editor\). It creates a new node with utterances which are negations of the original node utterances \(e.g. from node with utterance “I like that“ will be created a node containing an utterance “I don’t like that“\).

**NOTE**: The new nodes are created on top of the originals.

### Global Intent  <a id="global-intent"></a>

This node serves for starting a branch of a dialogue. One node labeled ‘Start’ is required for each dialogue, others are optional. In the headline, fill in the intent’s name \(must be unique in context of the current dialogue\). The content of this node are examples of user utterances which represent this intent \(e.g. fill in “What time it is“ or “Do you know the time“ for intent which prompts the bot to tell its user the time\). The exception is the start node which is empty.

| **Property name** | **Property type** | **Purpose** |
| :--- | :--- | :--- |
| Threshold | float | The confidence of the intent recognition must be higher than this value for the intent to activate |

### Sound  <a id="sound"></a>

Sound node is a special version of the Response node. The designer can write either an audio item from File Assets or a full URL pointing to audio resource file.

NOTE: Option “External” must be chosen in the dropdown menu above the text editor for the latter to work.

### Function  <a id="function"></a>

In case the dialogue needs to be directed programmatically, use this node. One link can go in and multiple out. Headline is the name of the function and should be unique.

The JS function should return the number of the next node. When the dialogue is loaded, the functions is included in the code on the “Functions“ tab.

### User Input  <a id="user-input"></a>

This node can also be filled with code similarly to Function node.

| **Property name** | **Property type** | **Purpose** |
| :--- | :--- | :--- |
| Disable global intents | boolean | If true, global intents will be disregarded when deciding the next state |

### Sub-dialogue  <a id="sub-dialogue"></a>

The node serves for adding a connection to a different dialogue model. Headline is the name of the subdialogue - must be valid existing dialogue in the system. Does not need to be unique \(same subdialogue can be called from different branches of the main dialogue\). Once the subdialogue finishes \(with StopDialogue node - see below\), the main dialogue continues with the branch linked to the lower port of this node.

### Global Action  <a id="global-action"></a>

### Action \(local\)  <a id="action-(local)"></a>

### Command  <a id="command"></a>

### Image  <a id="image"></a>

### Enter  <a id="enter"></a>

### Go Back  <a id="go-back"></a>

### Sleep  <a id="sleep"></a>

### Exit  <a id="exit"></a>

### End [\#](https://promethist.myjetbrains.com/youtrack/articles/APP-A-11/Node-Types#end) <a id="end"></a>

