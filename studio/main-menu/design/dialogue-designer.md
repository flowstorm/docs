---
description: >-
  The main editing environment for conversation designers. This is where you
  create, test, and modify your dialogues – the node graphs as well as the
  related code.
---

# Dialogues (editor)

## Parts of the editor

<figure><img src="../../../.gitbook/assets/Screenshot 2023-02-22 at 10.27.36.png" alt=""><figcaption></figcaption></figure>

1. **Tabs – different dialogues**\
   &#x20;   \- You can work with multiple dialogues at once. Anytime you create/open a new dialogue, it will open in a new tab.\
   &#x20;   \- Dialogue name in bold = unsaved changes\
   &#x20;   \- Checkmark = the currently opened version is successfully built
2.  **Tabs – current dialogue edits**

    &#x20;   \- **Graph** – edit the node graph and test the dialogue

    &#x20;   \- **Code** – edit the "init code" of the dialogue (this code is executed every time the dialogue is launched)

    &#x20;   \- **Properties** – change the properties of the dialogue (such as its name, the persona who should handle this dialogue, the dialogue background, the init mixins for the dialogue's init code...)

    &#x20;   \- **Description** – if you want, use this simple text editor to take notes in [Markdown](https://www.markdownguide.org/basic-syntax)

    &#x20;   \- **Source** – the source code of the dialogue model in JSON; you can find the dialogue ID under `_id`

    &#x20;   \- **Workflow** – here you can "publish" or "archive" the dialogue (both will lock the current version and make it non-editable), or see if anyone is currently working on it
3. **Quick actions (editing)**: hover over a button to see what it does
4. **Tabs – palettes and search**\
   &#x20;   \- **Nodes** – drag\&drop different types of nodes\
   &#x20;   \- **Nodes with mixins** – drag\&drop nodes with a pre-filled [mixin](dialogue-mixin.md) reference\
   &#x20;   \- **Files** – drag\&drop an image or a sound\
   &#x20;   \- **Snippets** – drag\&drop pre-made [snippets](snippet-designer.md)\
   &#x20;   \- **Search** – search across names and content in the dialogue graph
5. **Graph editing canvas**
6. **Tabs – selected node details**
7. **Tabs – technical logs and info** (most importantly _Build log_ and _Run log_)
8. **Test run & client attributes settings**
9. **Interactive side-panel**: where you can interact with the dialogue, also see [this article](../../interactive-side-panel.md)
10. **Quick actions (model)**: most importantly _Edit mode_ (click on <img src="../../../.gitbook/assets/image (5).png" alt="" data-size="line"> to be able to edit the dialogue), _Build_ (click on <img src="../../../.gitbook/assets/image (10).png" alt="" data-size="line"> to build the dialogue), _Save changes_, _Refresh_, or _Create a new version_\
    &#x20;   \- hover over a button to see what it does

## Controlling the graph canvas  <a href="#basic-controls" id="basic-controls"></a>

* Move the graph canvas by clicking and quickly dragging the mouse.
* Zoom the graph canvas by pressing Control and scrolling the mouse wheel.
* Select multiple nodes by clicking and holding the mouse and then dragging a selection box over the nodes.
* Copy and paste node structures via Ctrl+C and Ctrl+V (on Mac, Cmd+C and Cmd+V). Note that the duplicated nodes may appear on top of the original ones.
  * You can copy and paste between open windows of the editor, but make sure you have focus on the main editing area. If pasting does not work, try closing and reopening the tab or window you are attempting to paste to.

## Creating a node structure <a href="#working-with-designer-editor-tab" id="working-with-designer-editor-tab"></a>

A **dialogue graph** is created by dragging a node (or a group of nodes) from the palette on the left to the main editing area.

Transitions between nodes can be created by holding a mouse over a port (a small square visible when hovering the cursor over a node) and dragging the link to another port.

![](<../../../.gitbook/assets/drag-and-drop (4).gif>)

Find more conversation design tips in [Design a Dialogue](../../../how-to/design/).
