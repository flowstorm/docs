---
description: >-
  This guide describes how to attach the Promethist JS client to your web
  application.
---

# Web client

## Prerequisites

Please check out also [this article](https://docs.flowstorm.ai/core/client-integrations/web-socket) to better understand how the communication between client and server works.

## Linking the script

There are two ways to link the library to your application. For simpler apps, add this line to the head of your `index.html` file:

```markup
<script type="text/javascript" src=https://bot.flowstorm.ai/service.js></script>
```

Alternatively, you can use a package manager such as Yarn or NPM. Create a file named `.npmrc` containing this (or add to your existing one):

```javascript
@flowstorm:registry=https://gitlab.com/api/v4/projects/23512224/packages/npm/
```

Then, add the dependency:

```bash
yarn add @flowstorm/bot-service
# or
npm install @flowstorm/bot-service
```

## Instancing the bot

If you linked the package, in your HTML, add this to your JavaScript code:

```javascript
const Bot = window.botService.default
```

or if you used NPM/Yarn, add this:

```javascript
import Bot from '@flowstorm/bot-service';
```

Then, instantiate the bot object:

```javascript
const bot = Bot(
    'https://core.flowstorm.ai',
    'sender',
    true,       // autostart
    clientCallback,
    false,      // called from Kotlin
    undefined   // JWT token
    );
```

Arguments:

* **Core URL **(String) - URL of the back-end system
* **Device ID** (String)- Identifier of the client
* **Autostart** (Boolean) - Whether the bot should start the conversation immediately after opening the socket. If `false`, the conversation must be started manually by calling `bot.handleOnTextInput('#intro', false)` (see the "Controlling the bot" section).
* **Callback provider** (object) - contains functions which the bot executes in certain situations. See the "Implementing callbacks" section for complete specification.
* **Kotlin** (Boolean) - whether the bot is linked from Kotlin code
* **Token** (String) - JWT token identifying the user. If `undefined`, the conversation will start in anonymous mode

## Implementing callbacks

There are certain functions which are called in the bot code, but are not implemented there. They serve for allowing the front-end to react to certain events during the conversation. For the client to work correctly, they must be implemented in your web application code and passed to the bot object on creation. Attaching the functions to the object is done by calling e.g.

```javascript
const clientCallback = {};
// With arguments
clientCallback.setStatus = newState => {
    setState(newState); // Your method
};
// Without arguments
clientCallback.getVoice = this.getVoice; // this.getVoice = your method
```

The functions in your applications can have arbitrary name and, with the exception of the three `get `methods, may even be empty, but all the properties listed below must be defined and callable.

Below is the list of needed functions:

| **Name**      | **Parameters**                                                                                                                                                                                                                                                                                                       | **Called** **when**                                                                                              | **What is handled**                                                                                                                                                                                                                                                                                                 |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| setStatus     | **newState** - object with String field status                                                                                                                                                                                                                                                                       | <p>Whenever the bot </p><p>changes status</p>                                                                    | <p>Whenever the bot changes </p><p>status, this method is called </p><p>so that the front end can </p><p>reflect this. </p><p>Inspect <code>newState.status</code></p><p>to see the current bot status.</p><p>Note that you should keep track of the status on your app's level to use it in certain functions.</p> |
| addMessage    | <p><strong>type</strong> - String, “sent“ or “received” <br> <strong>text</strong> - String, content of the message<br><strong>image</strong> - optional String, URL of image<br><strong>background</strong> - optional String, URL of image</p><p><strong>signal</strong> - String, user signal starting with #</p> | <p>Whenever a message </p><p>comes (multiple times </p><p>a turn from bot, </p><p>once a turn from the user)</p> | <p>In this callback, </p><p>handle displaying </p><p>the message and image </p><p>if it is included.</p>                                                                                                                                                                                                            |
| addLogs       | **logs** - array of Strings                                                                                                                                                                                                                                                                                          | Once a turn                                                                                                      | <p>Print technical logs </p><p>from the backend</p>                                                                                                                                                                                                                                                                 |
| addVideo      | <p><strong>video</strong> - String, URL of the video</p><p><strong>callback</strong> - function to execute once the video finishes</p>                                                                                                                                                                               | If the turn contains a video                                                                                     | Display a video in the UI                                                                                                                                                                                                                                                                                           |
| onError       | **error** - object                                                                                                                                                                                                                                                                                                   | If an error occurs on the backend                                                                                | Handle the error                                                                                                                                                                                                                                                                                                    |
| onEnd         |                                                                                                                                                                                                                                                                                                                      | <p>When the </p><p>conversation ends</p>                                                                         | Reflect the end on the GUI                                                                                                                                                                                                                                                                                          |
| getAttributes |                                                                                                                                                                                                                                                                                                                      | In the beginning of the conversation                                                                             | <p>Return object with </p><p>client attributes such as </p><p>location or whether </p><p>the client has a screen</p>                                                                                                                                                                                                |
| getUUID       |                                                                                                                                                                                                                                                                                                                      | In the beginning of the conversation                                                                             | <p>Return session ID </p><p>(String in UUID format)</p>                                                                                                                                                                                                                                                             |
| getVoice      |                                                                                                                                                                                                                                                                                                                      | In the beginning of the conversation                                                                             | <p>Return voice name (String) </p><p>which should be used for the conversation (<code>undefined</code> </p><p>to use dialogue default)</p>                                                                                                                                                                          |
| focusOnNode   | **node** - Int, ID of node                                                                                                                                                                                                                                                                                           | Once per bot message                                                                                             | Used in editor to track the conversation progress in the dialogue tree                                                                                                                                                                                                                                              |
| handleCommand | <p><strong>command</strong> - String, command name</p><p><strong>code</strong> - String, command payload</p>                                                                                                                                                                                                         | Whenever there is a command in the dialogue                                                                      | Respond with the client to the command                                                                                                                                                                                                                                                                              |
| play          | **sound** - String                                                                                                                                                                                                                                                                                                   | On bot ready (if autostart is false)                                                                             | Play "bot ready" sound                                                                                                                                                                                                                                                                                              |



## Launching the bot

Start the conversation by calling

```javascript
bot.init(
    'xxxxxxxxxxxxxxxxxxxxxxx', 
    'xy',        // language
    true,        // input audio
    true,        // output audio
    '#intro',    // starting message
    true,        // mask signals
    ['error'],   // allowed sounds
    false,       // save session
)
```

Arguments:

* **Application key** (String) - Identifier of the dialogue which the client should conduct
* **Language** (String) - Two-letter code for the language for the conversation
* **Default input audio** (Boolean) - Whether the bot should listen to audio input. If `false`, the user will be able to communicate with the bot only by text.
* **Default output audio** (Boolean) - Whether the bot should play its utterances as audio. If `false`, no audio will be played, including the status sounds.
* **Starting message** (String) - Initializing signal for the conversation
* **Mask signals** (Boolean) - Whether the signals from user (such as `#intro` or `#silence`) should be displayed in the conversation log.
* **Allowed sounds** (Array of Strings) - List of status sounds which are allowed to play during the conversation. The available sounds are `intro`, `error`, `listening`, `recognized` and `sleep`.
* **Save session** (Boolean) - If the recording fails, the client might (based on this setting) save the session by turning off the input audio. The conversation then continues in text-only mode.

After calling this, the bot will open a socket to the server. If `autoStart` was set to true, the conversation will begin. After its first utterance, the browser will ask for microphone permission and if it is granted, it will listen for audio input. If the callbacks are empty, the communication will be only through audio.

## Controlling the bot

The bot can also be controlled by calling certain methods

```javascript
// With parameters
bot.click('LISTENING');
// Without parameters
bot.pause();
```

| **Method name**   | **Arguments**                                                                                        | **Purpose**                                                                                                     |
| ----------------- | ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| onStopClick       |                                                                                                      | Ends the conversation                                                                                           |
| pause             |                                                                                                      | <p>Pauses current bot </p><p>utterance (if playing)</p>                                                         |
| resume            |                                                                                                      | <p>Resumes current bot</p><p>utterance (if paused)</p>                                                          |
| inAudio           | **state** - current bot state (String)                                                               | Turns audio input on and off                                                                                    |
| outAudio          | **state** - current bot state (String)                                                               | Turns audio output on and off                                                                                   |
| handleOnTextInput | <p><strong>text</strong> - text to send (String)</p><p><strong>audioOn</strong> - true (boolean)</p> | <p>Sends text input to the bot</p><p> instead of audio</p>                                                      |
| click             | **state** - current bot state (String)                                                               | <p>Simulates button click, acts differretly based on state.</p><p>RESPONDING - skips the current utterances</p> |
