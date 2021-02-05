---
description: The most used and powerful part of the platform is the dialogue designer.
---

# Dialogue Designer

{% hint style="info" %}
To get into Dialogue Designer, click on **Design** item in the top menu and choose **Dialogue Designer** or just press key **"d"** as a **keyboard shortcut**.
{% endhint %}

You can open as many models for editing \(or observing\) as you want in paralel. Anytime you create or open a new dialogue model you will get a new tab with a new independent editor.

![](../../../.gitbook/assets/image%20%2818%29.png)

Open the **Design section in the top panel** and choose **Dialogue Designer** or just press key **"d" as a keyboard shortcut** and you can start prototyping.

The Promethist editor is a tool which serves for creating, saving and building the dialogue models. It is available at [https://promethist.app](https://promethist.app/). You need a Google account to access it.

In the main editing area, the designer creates the dialogue tree by dragging nodes there from the palette, linking them and filling them with text. Images and text or JSON files can be added in the other tabs in the left column. The model can be saved, loaded or uploaded to the system using the “Model“ menu. In the “Testing“ menu, you can test the model in a web client or create a link to the model for sharing. In QA and Functions tab, you can add aspects to the model.

Above the text area on the right side of the screen is a status bar which is used to display responses to designer’s actions such as saving and loading. It also displays a reminder that there are unsaved changes to the model. The model is **saved automatically** under the name temp-&lt;yourusername&gt; every 5 minutes.

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

It contains everything from model ID to used intents and mixins.

![](../../../.gitbook/assets/image%20%2827%29.png)

#### Workflow

As the name suggests an editor can publish or archive the dialogue model. The operation changes the state of the version from editable to uneditable state.

If someone else left the window with editor open you can **Force unlock** on this tab. 

![](../../../.gitbook/assets/image%20%2823%29.png)





