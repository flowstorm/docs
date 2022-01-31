---
description: >-
  Channel is a component which connects and enables exchange of verbal as well
  as other conversation related data between conversation-capable client
  software to the Flowstorm NLP pipeline.
---

# Channels

Flowstorm provides set of channel implementation in order to support integration of various client software (web and mobile apps, phone calls, voice asisstants and even other various conversation mediating systems).

There are two basic types of channel implementations

1. **External API** implementations used to provide endpoint for existing 3rd party conversation/verbal communication systems like IVA (Intelligent Voice Assistant) platforms or IVR (Interactive Voice Response) systems, including
   * [Amazon Alexa REST](amazon-alexa-rest.md)
   * [Google Assistant REST](google-app-rest.md)
   * [Twilio Stream Socket](twilio.md)
2. **Flowstorm Channel API** implementations leveraged by Flowstorm client apps and libraries as well as specific client implementations (e.g. web, mobile or 3D/VR apps) &#x20;
   * [Flowstorm Socket V1](flowstorm-sockets/web-socket.md) and&#x20;
   * [Flowstorm Socket V2](flowstorm-sockets/flowstorm-socket-v2.md)&#x20;

## Flowstorm Channel Events

Flowstorm Channel API socket implementations leverage set of events declared in platform independent module [channel/api](https://gitlab.com/promethistai/flowstorm/-/tree/master/channel/api/src/commonMain/kotlin/ai/flowstorm/channel) so that they can be used on both client and server side

| Name                         | Protocol Version  | Emittor                                           | Purpose                                                                                                                                                                                                                                                                                         |
| ---------------------------- | ----------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `InputEvent`                 | V1, V2            | Client                                            | Input object passed from client to server (e.g. when user is using text input OR ASR is being done on the client side). As soon as this event is emitted client should go to `Processing` state.                                                                                                |
| ~~`RequestEvent`~~           | V1 only, obsolete | Client                                            | Transport of obsolete request object from client to the server                                                                                                                                                                                                                                  |
| `InputAudioStreamOpenEvent`  | V1 only           | Client                                            | Indicates beginning of the input audio stream                                                                                                                                                                                                                                                   |
| `InputStreamCloseEvent`      | V2 only           | Client                                            | Used to indicate end of input stream when client user switches to  text input                                                                                                                                                                                                                   |
| `InputAudioStreamCloseEvent` | V1 only           | Client                                            | Indicates end of the input audio stream if client has received finally recognized input OR client user has switched to the text input                                                                                                                                                           |
| `RecognizedInputEvent`       | V2 only           | Server                                            | Used to pass client interim or final input object recognized from input stream                                                                                                                                                                                                                  |
| `RecognizedEvent`            | V1 only           | Server                                            | Used to pass client interim or final input text recognized from input audio stream                                                                                                                                                                                                              |
| `ResponseItemEvent`          | V1, V2            | Server                                            | Response item can contain text and/or audio, video or command code (payload) to be interpreted by the client immediately.                                                                                                                                                                       |
| `ResponseItem`               | V1, V2            | Server                                            | Indicates completion of the response (and optionally end of session). Client should go to and stay in the `Responding` state until it has finished all response item processing, then go to `Listening` state) or go to sleep (`Sleeping` state) depending on whether session has ended or not. |
| `InitEvent`                  | V1 only           | Client                                            | Used to initiate connection to the server                                                                                                                                                                                                                                                       |
| `ReadyEvent`                 | V1 only           | Server                                            | Acknowledge of initiated connection from the server                                                                                                                                                                                                                                             |
| `ErrorEvent`                 | V1, V2            | Server                                            | Non-recoverable server error text and source name. Client should display or otherwise indicate (e.g. by prerecorded voice message) server error and end session instantly by going to the `Sleeping` or `Failed` state.                                                                         |
| `SessionStartEvent`          | V1 only           | Server                                            | Acknowledge of  new session start (transport of session ID; V2 socket is using cookie for that)                                                                                                                                                                                                 |
| `SessionEndEvent`            | V1 only           | Server                                            | Indication of session end (client should discard session ID; obsolete, replaced with sessionEnded property of Response object transported via `ResponseEvent`)                                                                                                                                  |
| `LogEvent`                   | V1, V2            | Client                                            | Used to transport client's log entries to the server where they are stored to the current session                                                                                                                                                                                               |
| `BinaryEvent`                | V2 only           | <p>Client<br>Server potentially in the future</p> | Used to transport block of audio/video data from client to the server                                                                                                                                                                                                                           |

