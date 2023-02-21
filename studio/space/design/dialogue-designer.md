---
description: >-
  The main editing environment for conversation designers. This is where you
  create, test, and modify your dialogues – the node graphs as well as the
  related code.
---

# \*Dialogues (editor)

{% hint style="info" %}
You can work with multiple dialogues **** at once. Anytime you create/open a new dialogue, it will open in a **new tab**.
{% endhint %}

## Graphical user interface

The **Graph** view provides you with the powerful user interface that allows model creating & editing. Elements can be added and organized by drag & drop interface. The right panel allows you to fill the content, code (where applicable) and/or set the node preferences like Label, Name, references to Mixins and Entities.

## Tabs

In the following picture, you can see the editor with two dialogues opened.

* `[#examples]habits/relax:1`
  * **active tab** (white background)
  * **unsaved changes** (the title is in bold)
  * **editing mode is off** (the Edit icon on the left <img src="../../../.gitbook/assets/Screenshot 2023-02-21 at 18.46.23.png" alt="" data-size="line"> is inactive & the grid behind the canvas is hidden)
* `[#examples]pre-example/example:1`
  * **inactive tab** (blue background)
  * **all changes saved** (title not in bold)
  * **current version is built** (checkmark next to the title)

![Editor in the view mode](<../../../.gitbook/assets/image (78).png>)

Compare the edit mode in the picture below

![Editor in the edit mode](<../../../.gitbook/assets/image (79).png>)

## Basic controls  <a href="#basic-controls" id="basic-controls"></a>

Main area/canvas can be dragged by clicking and moving a mouse or zoomed by holding **Control** and scrolling the mouse wheel. Multiple nodes can be selected by holding a mouse button then dragging a select box over them. Nodes or groups of nodes and links can be duplicated by the shortcuts **Control + C** and **Control + V** (**Command** on Mac) ; however, note that the duplicated nodes will appear on top of the originals so they won’t be immediately visible. The designer can then drag them to a desired position. The copying can be done between open windows of the editor, but make sure you have focus on the main editing area. If the pasting does not work, try closing and reopening the tab or window you are attempting to paste to.

## Working with designer editor tab  <a href="#working-with-designer-editor-tab" id="working-with-designer-editor-tab"></a>

#### Graph - Main editing area and palette  <a href="#graph---main-editing-area-and-palette" id="graph---main-editing-area-and-palette"></a>

A **dialogue graph** is created by dragging node (or a group of nodes) from the palette on the left to the main editing area.&#x20;

Transitions between nodes can be created by holding a mouse over a port (small squeare visible when putting the mouse cursor over a node) and dragging the link to another port.

![](<../../../.gitbook/assets/drag-and-drop (4).gif>)

Would you like to know how to create a complex and/or a robust dialogue? See articles in the categories reflecting the process of the application creation and maintenance

* [Design a Dialogue](../../../how-to/design/)
* [Create an Application](../../../how-to/applications/)
* [Analyze Your Traffic](broken-reference)
* [Collaborate with other Designers](broken-reference)

#### Code  <a href="#code" id="code"></a>

In this tab, you can implement the code that will be run before starting the conversation.

![](<../../../.gitbook/assets/image (20).png>)

More about [DialogueScript](../../../development/dialoguescript/)

#### Properties  <a href="#properties" id="properties"></a>

Configure the dialogue settings typically voice used for the dialogue pronunciation - choose Language and one of the Voices available in the prefered language.

![](<../../../.gitbook/assets/image (21).png>)

#### Description

Description tab is for the description and/or documentation of the dialog model. Please use [Markdown format](https://www.markdownguide.org/basic-syntax) for that.

#### Source

Overview of the dialogue model structure in JSON format.&#x20;

It contains everything from model ID to used intents and [mixins](dialogue-mixin.md).

![](<../../../.gitbook/assets/image (27).png>)

#### Workflow

{% hint style="info" %}
The lifecycle of the dialogue: _New_ -> _Draft_ -> _Published_ -> _Archived_
{% endhint %}

The workflow of your specific dialogue can be different. Let's have a look at each state.

* New - unsaved
* Draft - saved, editable, can be shared via an application
* Published - saved, non-editable
* Archived - saved, non-editable

If someone else left the window with editor open this is the screen where you can **Force unlock** on this tab.&#x20;

![](<../../../.gitbook/assets/image (23).png>)

## Related articles

* [Dialogue Model](../../../development/dialogue-model-coding/building-blocks/dialogue-model.md)
* [Node Types](../../../development/dialogue-model-coding/building-blocks/node-types.md)
* [Dialogue Mixins](dialogue-mixin.md)
* [Snippets](../../../development/dialogue-model-coding/building-blocks/snippets.md)
* [File Assets](broken-reference)
* [Voices](broken-reference)
* [Applications](../../../development/dialogue-model-coding/building-blocks/applications.md)
* [Personas](../../../development/dialogue-model-coding/building-blocks/personas.md)
