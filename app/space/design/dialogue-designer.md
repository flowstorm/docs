---
description: 'The dialogue designer allows us to create, edit and publish dialogue models.'
---

# Dialogue Designer

The Flowstorm editor is a tool which serves for creating, saving and building the dialogue models. It is available at [https://app.flowstorm.ai](https://app.flowstorm.ai). 

{% hint style="info" %}
To get into Dialogue Designer, click on **Design** item in the top menu and choose **Dialogue Designer** or just press key **"d"** as a **keyboard shortcut**.
{% endhint %}

You can **open** as many **models for editing** \(or observing\) as you want in paralel. Anytime you create or open a new dialogue model you will get a **new tab** with a new **independent editor**.

### Tabs - Editors

In the following picture you can see the admin with two editors opened.

{% hint style="info" %}
A tabs reflect the statuses and changes of the dialogue models.
{% endhint %}

* **The first one** \(habits/relax\)
  * **Active tab** - white background
  * **Edited** - title is in **bold font**
  * **User is not editing now** - the **edit icon** on the left is **inactive** & the **grid** behind the canvas is hidden
  * **Not built** yet - missing checkmark
* **The second one** \(pre-example/example\)
  * **Built - checkmark** next to the title
  * **Not edited** - title is not in **bold font**

![Editor in the view mode](../../../.gitbook/assets/image%20%2878%29.png)

Compare the edit mode in the picture below

![Editor in the edit mode](../../../.gitbook/assets/image%20%2879%29.png)

### Basic controls  <a id="basic-controls"></a>

Main area/canvas can be dragged by clicking and moving a mouse or zoomed by holding **Control** and scrolling the mouse wheel. Multiple nodes can be selected by holding a mouse button then dragging a select box over them. Nodes or groups of nodes and links can be duplicated by the shortcuts **Control + C** and **Control + V** \(**Command** on Mac\) ; however, note that the duplicated nodes will appear on top of the originals so they won’t be immediately visible. The designer can then drag them to a desired position. The copying can be done between open windows of the editor, but make sure you have focus on the main editing area. If the pasting does not work, try closing and reopening the tab or window you are attempting to paste to.

### Working with designer editor tab  <a id="working-with-designer-editor-tab"></a>

#### Graph - Main editing area and palette  <a id="graph---main-editing-area-and-palette"></a>

The dialogue trees are created by dragging node from the palette to the main editing area and connecting them. The nodes without pre-filled utterances need to be edited. This is done by clicking the node whose contents then appear on the right in the text editor and the designer can update them. Headline of a node can be edited by double clicking it \(text editor is not involved\).

Links can be created by holding a mouse over a port \(small circle visible when putting the mouse cursor over a node\) and dragging the link to another port. Links can only be created from a bottom or side \(in case of function node\) port to a top port.

#### Code  <a id="code"></a>

In this tab, you can implement the code that will be run before starting the conversation.

![](../../../.gitbook/assets/image%20%2820%29.png)

#### Properties  <a id="properties"></a>

Configure the dialogue settings typically voice used for the dialogue pronunciation - choose Language and one of the Voices available in the prefered language.

![](../../../.gitbook/assets/image%20%2821%29.png)

#### Description

Description tab is for the description and/or documentation of the dialog model. Please use [Markdown format](https://www.markdownguide.org/basic-syntax) for that.

#### Source

Overview of the dialogue model structure in JSON format. 

It contains everything from model ID to used intents and [mixins](dialogue-mixin.md).

![](../../../.gitbook/assets/image%20%2827%29.png)

#### Workflow

{% hint style="info" %}
The lifecycle of the dialogue: _New_ -&gt; _Draft_ -&gt; _Published_ -&gt; _Archived_
{% endhint %}

The workflow of your specific dialogue can be different. Let's have a look at each state.

* New - unsaved
* Draft - saved, editable, can be shared via an application
* Published - saved, non-editable
* Archived - saved, non-editable

If someone else left the window with editor open this is the screen where you can **Force unlock** on this tab. 

![](../../../.gitbook/assets/image%20%2823%29.png)

### Related articles

* 
