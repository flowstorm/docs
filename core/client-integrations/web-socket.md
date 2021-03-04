---
description: >-
  This page describes communication protocol between clients based on Common
  Client library and the server socket implemented as a part of Runner
  component.
---

# Web Socket

Web Socket is provided by [Core Runner](../core-services/runner.md) service on URI `/socket/` 

The Platform Cloud Core Runner service provides web socket on URL address  [wss://core.promethist.com/socket/](wss://core.promethist.com/socket/) 

## States 

Client has defined following states

| Name | Description |
| :--- | :--- |
| `Closed` | connection to server is not open \(this is the initial state of the client\) |
| `Open` | connection established |
| `Failed` | connection failed - in that case client should repeatedly try to connect; in common implementation, every 10 seconds by default |
| `Sleeping` | no conversation session is going on \(session has not started yet or session has been finished\) |
| `Listening` | bot is waiting for user input \(input audio and/or text, depending on client configuration\) |
| `Processing` | user input has been received by server and is being processed by server |
| `Responding` | response is sent to the client - after all the output audio \(speech synthesis\) is played, client goes back to `Listening` state |

## Events

Client exchanges with server JSON messages representing event objects of class `BotEvent`

```kotlin
open class BotEvent {
    data class Init(val key: String, val deviceId: String, val token: String? = null, val config: BotConfig) : BotEvent()
    class Ready : BotEvent()
    data class Error(val text: String) : BotEvent()
    data class Request(val request: CoreRequest) : BotEvent()
    data class Response(val response: CoreResponse) : BotEvent()
    data class Recognized(val text: String) : BotEvent()
    data class SessionStarted(val sessionId: String) : BotEvent()
    class SessionEnded : BotEvent()
    class InputAudioStreamOpen : BotEvent()
    class InputAudioStreamClose : BotEvent()
}
```

If defines following event types to be used by **client**

| Name | Description |
| :--- | :--- |
| `Init` | to initialise connection to server |
| `Request` | to send user request |
| `InputAudioStreamOpen` | to initiate input audio processing \(ASR\) |
| `InputAudioStreamClose` | to finish input audio processing |
| `InputAudioStreamCancel` | to cancel input audio prematurely |

Following event types are emitted by **server**

| Name | Description |
| :--- | :--- |
| `Ready` | to acknowledge initialisation from server side |
| `Response` | to send bot response |
| `Recognized` | to send text result of ASR |
| `SessionStarted` | to pass valid session ID to the client |
| `SessionEnded` | to inform client that session has ended and session ID is no more valid and should be forget |

## Communication Flow

The logic of client to server communication is following

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Client</b>
      </th>
      <th style="text-align:left"><b>Server</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Connection initiation or restoration</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">State <code>Closed</code> immediately after start</td>
      <td style="text-align:left">waiting for new connections</td>
    </tr>
    <tr>
      <td style="text-align:left">State <code>Open</code> when web socket connection established to the server</td>
      <td
      style="text-align:left">creates BotSocketAdapter object and waits for events</td>
    </tr>
    <tr>
      <td style="text-align:left">Sending event <code>BotEvent.Init</code> containing app key, sender identification
        and client requirements in object of type <a href="https://develop.promethist.ai/apidoc/client-app-common/ai.promethist.client/-bot-client-requirements/index.html">BotClientRequirements</a>
      </td>
      <td style="text-align:left">verifies sender, if ok then sends <code>BotEvent.Ready</code> otherwise <code>BotEvent.Error</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Accepting events</p>
        <ul>
          <li><code>BotEvent.Ready</code> &gt; going to state <code>Sleeping</code>
          </li>
          <li><code>BotEvent.Error</code> &gt; showing error message text and going to
            state <code>Failed</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>Conversation</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>When client reaches state <code>Sleeping</code> after start, it will be
          waiting for user input (pushing device button or keyboard ENTER key) OR
          (if auto start config param is set) will send event <code>BotEvent.Request</code> containing
          request object of class <a href="https://develop.promethist.ai/apidoc/core-api/com.promethist.core/-request/index.html">Request</a> with
          user <code>input.transcript = &quot;#intro&quot;</code>
        </p>
        <p>version 2 expects that client propose sessionId (in opposite to v1 where
          sessionId was defined strictly by server) - server can accept it or set
          different - in both cases it will send <code>BotEvent.SessionStarted</code> event
          containing valid conversation sessionId to the client. This feature will
          allow to transfer user session from one client to another in the future
          (e.g. to go from hardware device to mobile and back)</p>
      </td>
      <td style="text-align:left">sends event <code>BotEvent.SessionStarted</code> containing accepted or
        different <code>sessionId</code> which will be used for conversation session
        identification; afterwards starts with processing request and sending <code>BotEvent.Response</code> containing
        response object of class <a href="https://develop.promethist.ai/apidoc/core-api/com.promethist.core/-response/index.html">Response</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Sending text messages = sending event <code>BotEvent.Request</code> containing
        request object of class <a href="https://develop.promethist.ai/apidoc/core-api/com.promethist.core/-request/index.html">Request</a> with
        user <code>input.transcript = &quot;user text input&quot;</code>
      </td>
      <td style="text-align:left">processing request and sending <code>BotEvent.Response</code> containing
        response object of class <a href="https://develop.promethist.ai/apidoc/core-api/com.promethist.core/-response/index.html">Response</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Accepting <code>BotEvent.Response</code> events going to the state <code>Responding</code> -
          depending on client configuration, response text can be shown, audio (speech
          synthesis or another audio track specified in response) can be played and/or
          image/video can be shown from URL(s) specified in response. When output
          is finished, clients goes to state <code>Listening</code>
        </p>
        <p>Server can also actively send <code>BotEvent.Response</code> when client
          is <code>Sleeping</code> to initiate conversation triggered by external circumstances
          (e.g. certain scheduled time, web hook etc.)</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Audio Input for Speech-To-Text</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">When client is in Listening state and audio input is enabled, it sends
        event <code>BotEvent.InputAudioStreamOpen</code>
      </td>
      <td style="text-align:left">Server confirms to be ready to intercept and process audio input stream
        returning <code>BotEvent.InputAudioStreamOpen</code> event</td>
    </tr>
    <tr>
      <td style="text-align:left">Client streams audio binary data to the server</td>
      <td style="text-align:left">Server reads audio binary data passing them to the ASR (STT) service.
        When ASR is finished it sends <code>BotEvent.Recognized</code> event containing
        text transcript to the client and starts processing it</td>
    </tr>
    <tr>
      <td style="text-align:left">Client accepts event <code>BotEvent.Recognized</code> optionally showing
        text transcript to the user, going to state Processing. It also sends event
        BotEvent.InputAudioStreamClose to notify server that audio input has been
        closed on client side</td>
      <td style="text-align:left">Server release resources used to process input audio</td>
    </tr>
  </tbody>
</table>

## Example of client-server communication

### Connection Initiation

Client establishes web socket connection and is in `Open` state, so it can initiate it for future communication by `Init` event containing config parameters describing how STT \(Speech-To-Text\) and STT \(Text-To-Speech\) will be treated.

```javascript
{
  "type": "Init",
  "key": "5ea05091a7a6757defffa479",
  "deviceId": "standalone_3C22FBBBAD22",
  "config": {
    "locale": "en",
    "zoneId": "Europe/Prague",
    "sttMode": "SingleUtterance",
    "sttSampleRate": 16000,
    "tts": "RequiredLinks",
    "returnSsml": false,
    "silenceTimeout": 5000
  }
}
```

Server confirms that it is ready for communication by `Ready` event, client goes to `Sleeping` state

```javascript
{"type": "Ready"}
```

### **Conversation Request**

**Client** initiates conversation upon user activity \(e.g. pressing talk button\) by `Request` event containing `#intro` action

{% hint style="info" %}
Client can generate or set`sessionId` so it can attach to existing / previous session.
{% endhint %}

{% hint style="info" %}
`deviceId` should be unique per hardware + software client combination \(e.g. browser type with unique ID stored in local storage for web application, application ID + mobile phone ID for mobile clients, client software name combined with MAC address for standalone client appliances etc.\)
{% endhint %}

```javascript
{
  "type": "Request",
  "request": {
    "appKey": "5ea05091a7a6757defffa479",
    "deviceId": "standalone_3C22FBBBAD22",
    "sessionId": "abe55b84-2b6a-47bb-9e71-e12da1252321",
    "input": {
      "locale": "en_US",
      "zoneId": "Europe/Prague",
      "transcript": {
        "text": "#intro"
      }
    },
    "attributes": {
      "clientType": "standalone:1.0.0-SNAPSHOT"
    }
  }
}
```

{% hint style="info" %}
Request can contain attributes describing client current state, e.g. client type, location, temperature, etc. To understand better how these client attributes can be used in server part of Platform programming model please visit [Client Attributes](../../model/dialoguescript/attributes/client-attributes.md) page.
{% endhint %}

### **Conversation Response**

**Server** responds by `Response` event and client goes to `Responding` state, playing output audio

```javascript
{
  "type": "Response",
  "response": {
    "locale": "en",
    "items": [
      {
        "text": "What can I do for you, Tomas?",
        "ssml": null,
        "confidence": 1.0,
        "audio": "https://core.promethist.com/file/tts/18e77858dc3701a543732d0962c9b5bf.mp3",
        "ttsConfig": {
          "provider": "Amazon",
          "locale": "en_US",
          "gender": "Female",
          "name": "Joanna",
          "engine": "neural"
        },
        "repeatable": true
      }
    ],
    "sessionEnded": false,
    "sleepTimeout": 0
  }
}
```

### Speech Recognition

**Client** finished playing audio, it opens input audio by sending `InputAudioStreamOpen` event

```javascript
{"type": "InputAudioStreamOpen"}
```

**Server** confirms that audio stream is open by sending `InputAudioStreamOpen` event in return. Client goes to `Listening` state and starts to send binary audio packets to the sockets.

```javascript
{"type": "InputAudioStreamOpen"}
```

**Server** recognises speech in the input audio and sends it back to the client in `Recognized` event \(so it can display input transcript to the user\), client goes to `Processing` state

```javascript
{"type": "Recognized", "text": "tell me about this place"}
```

**Server** generates response and sends it in `Response` event, client goes to `Responding` state, plays response audio \(and optionally also displays response text\)

```javascript
{
  "type": "Response",
  "response": {
    "locale": "en",
    "items": [
      {
        "text": "Sorry, I can't see where you are located.",
        "ssml": null,
        "confidence": 1.0,
        "image": null,
        "video": null,
        "audio": "http://core.promethist.com/file/tts/83afc721a3c36afd8acd12f027a19023.mp3",
        "code": null,
        "background": "",
        "ttsConfig": {
          "provider": "Amazon",
          "locale": "en_US",
          "gender": "Female",
          "name": "Joanna",
          "engine": "neural"
        },
        "repeatable": true
      }
    ],
    "sessionEnded": true,
    "sleepTimeout": 0
  }
}

```

### Conversation End

As `sessionEnded` is set `true`, client goes do `Sleeping` mode and `sessionId` value is discarded \(e.g. set to `null`\) so new value will have to be created for new conversation. If `sleepTimeout` is non zero, then client also goes to `Sleeping` mode but keeps `sessionId` until sleep timeout expires. This allows to get back into the same session and have multiple conversations in the same session context.

