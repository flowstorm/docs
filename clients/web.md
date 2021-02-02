---
description: >-
  This guide describes how to attach the Promethist JS client to your web
  application.
---

# Web client

## Linking the script

Add this line to the head of your `index.html` file:

```markup
<script type="text/javascript" src=https://repository.promethist.ai/dist/bot/default/service.js></script>
```

## Instancing the bot

In your JavaScript code, add this:

```javascript
const Bot = window.botService.default

const bot = Bot(
    'https://port.promethist.com',
    'sender',
    true,       // autostart
    false,      // called from Kotlin
    undefined   // JWT token
    );
```

Arguments:

* **Port URL** - URL of the Promethist back-end
* **Sender** - Identifier of the client
* **Autostart** - Whether the bot should start the conversation immediately after opening the socket
* **Kotlin** - whether the bot is linked from Kotlin code
* **Token** - JWT token

## Implementing callbacks

There are certain functions which are called in the bot code, but are not implemented there. They serve for allowing the front-end to react to certain events during the conversation. For the client to work correctly, they must be implemented in your web application code and attached to the bot variable. The attaching is done by calling e.g.

```javascript
// With arguments
bot.setStatus = newState => {
    setState(newState);
};
// Without arguments
bot.getVoice = this.getVoice;
```

The functions in your applications can have arbitrary name and may even be empty, but all “bot.x” properties listed below must be defined and callable.

Below is the list of needed functions:

| **Name** | **Parameters** | **Called** **when** | **What is handled** |
| :--- | :--- | :--- | :--- |
| setStatus | **newState** - object with String field status | Whenever the bot changes status | Whenever the bot changes status, this method is called so that the front end can reflect this. Inspect newState.status to see the current bot status |
| getStatusString | **status** - object |  |  |
| addMessage | **type** - String, “sent“ or “received”   **text** - String, content of the message **image** - optional String, URL of image **background** - optional String, URL of image | Whenever a message comes \(multiple times a turn from bot, once a turn from the user\) | In this callback, handle displaying the message and image if it is included. |
| addLogs | **logs** - array of Strings | Once a turn | Print technical logs from the backend |
| onError | **error** - object | If an error occurs on the backend | Handle the error |
| onEnd |  | When the conversation ends | Reflect the end on the GUI |
| getAttributes |  | In the beginning of the conversation | Send object with client attributes such as location or whether the client has a screen |
| getUUID |  | In the beginning of the conversation | Return session ID in UUID format |
| getVoice |  | In the beginning of the conversation | Return voice which should be used for the conversation \(undefined to use dialogue default\) |
| focusOnNode |  | Once per bot message | Used in editor to track the conversation progress in the dialogue tree |
| play | **sound** | When starting and ending listening, on error and on bot ready \(when autostart is false\) | Play indicator sounds |

## Launching the bot

Start the conversation by calling

```javascript
bot.init(
    'xxxxxxxxxxxxxxxxxxxxxxx', 
    'xy',       // language
    true,       // input audio
    true,       // output audio
    '#intro'    // starting message
)
```

Arguments:

* **Application key** - Identifier of the dialogue which the client should conduct
* **Language** - Two-letter code for the language for the conversation
* **Default input audio** - Whether the bot should listen to audio input
* **Default output audio** - Whether the bot should play its utterances as audio
* **Starting message** - Initialising signal for the conversation

## Controlling the bot

The bot can also be controlled by dispatching events on a document.

| **Event name** | **Arguments** | **Purpose** |
| :--- | :--- | :--- |
| BotStopEvent |  | Ends the conversation |
| BotPauseEvent |  | Pauses current bot utterance \(if playing\) |
| BotResumeEvent |  | Resumes current bot utterance \(if paused\) |
| InputAudioEvent |  | Turns audio input on and off |
| OutputAudioEvent |  | Turns audio output on and off |
| TextInputEvent | text: text message from the user audioOn | Sends text input to the bot instead of audio |
| SLEEPINGClickEvent |  | Launches the bot |
| RESPONDINGClickEvent |  | Skips current bot utterances |

