---
description: >-
  This guide describes how to attach the Promethist JS client to your web
  application.
---

# Web client

## Prerequisites

Please check out also [this article](https://docs.flowstorm.ai/core/client-integrations/web-socket) to better understand how the communication between client and server works.

## Linking the script

Add this line to the head of your `index.html` file:

```markup
<script type="text/javascript" src=https://repository.promethist.ai/dist/bot-service.js></script>
```

## Instancing the bot

In your JavaScript code, add this:

```javascript
const Bot = window.botService.default

const bot = Bot(
    'https://core.promethist.com',
    'sender',
    true,       // autostart
    false,      // called from Kotlin
    undefined   // JWT token
    );
```

Arguments:

* **Port URL** \(Boolean\) - URL of the back-end system
* **Sender** \(String\)- Identifier of the client
* **Autostart** \(Boolean\) - Whether the bot should start the conversation immediately after opening the socket. If `false`, the conversation must be started by dispatching `SLEEPINGClickEvent` on the document \(see the "Controlling the bot" section\).
* **Kotlin** \(Boolean\) - whether the bot is linked from Kotlin code
* **Token** \(String\) - JWT token identifying the user. If `undefined`, the conversation will start in anonymous mode

## Implementing callbacks

There are certain functions which are called in the bot code, but are not implemented there. They serve for allowing the front-end to react to certain events during the conversation. For the client to work correctly, they must be implemented in your web application code and attached to the bot object before starting the conversation. The function attaching is done by calling e.g.

```javascript
// With arguments
bot.setStatus = newState => {
    setState(newState); // Your method
};
// Without arguments
bot.getVoice = this.getVoice; // this.getVoice = your method
```

The functions in your applications can have arbitrary name and, with the exception of the three `get` methods, may even be empty, but all `bot.x` properties listed below must be defined and callable.

Below is the list of needed functions:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Name</b>
      </th>
      <th style="text-align:left"><b>Parameters</b>
      </th>
      <th style="text-align:left"><b>Called</b>  <b>when</b>
      </th>
      <th style="text-align:left"><b>What is handled</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">setStatus</td>
      <td style="text-align:left"><b>newState</b> - object with String field status</td>
      <td style="text-align:left">
        <p>Whenever the bot</p>
        <p>changes status</p>
      </td>
      <td style="text-align:left">
        <p>Whenever the bot changes</p>
        <p>status, this method is called</p>
        <p>so that the front end can</p>
        <p>reflect this.</p>
        <p>Inspect <code>newState.status</code>
        </p>
        <p>to see the current bot status</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">addMessage</td>
      <td style="text-align:left"><b>type</b> - String, &#x201C;sent&#x201C; or &#x201C;received&#x201D;
        <br
        /> <b>text</b> - String, content of the message
        <br /><b>image</b> - optional String, URL of image
        <br /><b>background</b> - optional String, URL of image</td>
      <td style="text-align:left">
        <p>Whenever a message</p>
        <p>comes (multiple times</p>
        <p>a turn from bot,</p>
        <p>once a turn from the user)</p>
      </td>
      <td style="text-align:left">
        <p>In this callback,</p>
        <p>handle displaying</p>
        <p>the message and image</p>
        <p>if it is included.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">addLogs</td>
      <td style="text-align:left"><b>logs</b> - array of Strings</td>
      <td style="text-align:left">Once a turn</td>
      <td style="text-align:left">
        <p>Print technical logs</p>
        <p>from the backend</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">onError</td>
      <td style="text-align:left"><b>error</b> - object</td>
      <td style="text-align:left">If an error occurs on the backend</td>
      <td style="text-align:left">Handle the error</td>
    </tr>
    <tr>
      <td style="text-align:left">onEnd</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>When the</p>
        <p>conversation ends</p>
      </td>
      <td style="text-align:left">Reflect the end on the GUI</td>
    </tr>
    <tr>
      <td style="text-align:left">getAttributes</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">In the beginning of the conversation</td>
      <td style="text-align:left">
        <p>Return object with</p>
        <p>client attributes such as</p>
        <p>location or whether</p>
        <p>the client has a screen</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">getUUID</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">In the beginning of the conversation</td>
      <td style="text-align:left">
        <p>Return session ID</p>
        <p>(String in UUID format)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">getVoice</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">In the beginning of the conversation</td>
      <td style="text-align:left">
        <p>Return voice name (String)</p>
        <p>which should be used for the conversation (<code>undefined</code> 
        </p>
        <p>to use dialogue default)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">focusOnNode</td>
      <td style="text-align:left"><b>node</b> - Int, ID of node</td>
      <td style="text-align:left">Once per bot message</td>
      <td style="text-align:left">Used in editor to track the conversation progress in the dialogue tree</td>
    </tr>
    <tr>
      <td style="text-align:left">play</td>
      <td style="text-align:left"><b>sound</b>
      </td>
      <td style="text-align:left">On bot ready (if autostart is false)</td>
      <td style="text-align:left">Play &quot;bot ready&quot; sound</td>
    </tr>
  </tbody>
</table>



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

* **Application key** \(String\) - Identifier of the dialogue which the client should conduct
* **Language** \(String\) - Two-letter code for the language for the conversation
* **Default input audio** \(Boolean\) - Whether the bot should listen to audio input. If `false`, the user will be able to communicate with the bot only by text.
* **Default output audio** \(Boolean\) - Whether the bot should play its utterances as audio. If `false`, no audio will be played, including the status sounds.
* **Starting message** \(String\) - Initializing signal for the conversation

After calling this, the bot will attempt to start the dialogue. After its first utterance, the browser will ask for microphone permission and if it is granted, it will listen for audio input. If the callbacks are empty, the communication will be only through audio.

## Controlling the bot

The bot can also be controlled by dispatching events on a document.

```javascript
// With parameters
document.dispatchEvent(new Event('BotStopEvent'))
// Without parameters
document.dispatchEvent(new CustomEvent('TextInputEvent', { detail: { audioOn: audioOn, text: text } }))
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Event name</b>
      </th>
      <th style="text-align:left"><b>Arguments</b>
      </th>
      <th style="text-align:left"><b>Purpose</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BotStopEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Ends the conversation</td>
    </tr>
    <tr>
      <td style="text-align:left">BotPauseEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>Pauses current bot</p>
        <p>utterance (if playing)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BotResumeEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>Resumes current bot</p>
        <p>utterance (if paused)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">InputAudioEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Turns audio input on and off</td>
    </tr>
    <tr>
      <td style="text-align:left">OutputAudioEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Turns audio output on and off</td>
    </tr>
    <tr>
      <td style="text-align:left">TextInputEvent</td>
      <td style="text-align:left">text: text message from the user
        <br />audioOn</td>
      <td style="text-align:left">
        <p>Sends text input to the bot</p>
        <p>instead of audio</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SLEEPINGClickEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Launches the bot</td>
    </tr>
    <tr>
      <td style="text-align:left">RESPONDINGClickEvent</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>Skips current bot utterances</p>
        <p>and goes to LISTENING state</p>
      </td>
    </tr>
  </tbody>
</table>

