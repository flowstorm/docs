---
description: >-
  Socket V2 is a brand new way how to communicate and integrate speech-capable
  client software to Flowstorm very efficiently.
---

# Flowstorm Socket V2

In fact, Socket V2 is not based on web socket technology, instead it leverages common HTTP GET/POST requests in a fashion so called HTTP long polling (so clients might wait for the complete response from the server for a variable amount of time, depending on duration of conversational turn processing). That makes communication protocol more simple and reliable as conversational interaction is (mostly) linear, HTTP requests also allows only to  write and read channel events alternately, but not simultaneously.&#x20;

Socket V2 implementation if provided by Core Application at single endpoint (`/client`) and supports two HTTP methods

* **PUT** for writing
  1. input and reading response of the conversational turn (that means user/client input representing object or audio/video input stream data, followed by finally recognized input and response items)
  2. other client related data to the server (e.g. client logs)
* **GET** for reading interim recognized input results and to get information that recognition has been finished (by end of response to that request) in [Audio/Video input streaming](flowstorm-socket-v2.md#audio-video-input-streaming).

{% hint style="info" %}
To understand how client using V2 socket can be implemented please you can take a look on source code of platform-independent [ClientV2](https://gitlab.com/promethistai/flowstorm/-/blob/master/client/lib/src/commonMain/kotlin/ai/flowstorm/client/ClientV2.kt) class as well as [JavaHttpSocketClient](https://gitlab.com/promethistai/flowstorm/-/blob/master/common/client/src/main/kotlin/ai/flowstorm/common/client/JavaHttpSocketClient.kt) class showing Java based client implementation of HTTP polling socket (interface [HttpSocketClient](https://gitlab.com/promethistai/flowstorm/-/blob/master/common/lib/src/commonMain/kotlin/ai/flowstorm/common/transport/HttpSocketClient.kt)). &#x20;
{% endhint %}

## Data formats&#x20;

### Audio/Video formats

Currently, V2 Socket support Audio/Video input in one of following formats

|                 | Content Type |
| --------------- | ------------ |
| Raw PCM audio   | audio/basic  |
| WAV audio file  | audio/wav    |
| MP3 audio file  | audio/mpeg   |

### Request/response content types

V2 sockets supports several input format (specified in `Content-Type` header) for requests:

* Plain text (`Content-Type: plain/text`) allowing only simple text input from user
* Stream of JSON objects representing one or more V2 client channel events (`Content-Type: application/json`)&#x20;
  * `InputEvent`
  * `BinaryEvent`
  * `InputStreamEvent`
* Audio/Video input (see list of supported [Audio/Video formats](flowstorm-socket-v2.md#audio-video-formats))

Server will respond with JSON objects representing output events, except case of plain text request when response will be also in plain text (see descripton of [plain text communication](flowstorm-socket-v2.md#plain-text) below). Here is the list of supported output events

* `RecognizedInputEvent` (GET and PUT)
* `ResponseItemEvent` (PUT only)
* `ResponseEvent` (PUT only)
* `ErrorEvent` (GET and PUT)

Please note that server will generate response continuosly so it may take some time between &#x20;

{% hint style="info" %}
See the detailed list of [Channel Events](../channel-events.md)&#x20;
{% endhint %}

## Configuration and session ID handling

V2 protocol is using HTTP request query parameters OR extra request headers to configure pipeline. You have to pass them in every turn as it is not guaranteed that subsequent turns in the same session will be processed by the same pipeline instance.&#x20;

Here is the full list of supported configuration values

| Request Header  | Query Parameter | Default Value (empty if mandatory) | Options / comments                                                                                                                        |
| --------------- | --------------- | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| X-Key           | key             |                                    | `applicationId` or `dialogue:dialogueId` (go to Studio to get it)                                                                         |
| X-DeviceId      | deviceId        |                                    | Your client device unique identifier                                                                                                      |
| Accept-Language | -               | en-US                              | See IETF language tag specification                                                                                                       |
| X-TimeZone      | timeZone        | Europe/Prague                      |                                                                                                                                           |
| X-SttMode       | sttMode         | SingleUtterance                    | SingleUtterance only supported by V2                                                                                                      |
| X-SttModel      | sttModel        | General                            | See [SttModel](https://gitlab.com/promethistai/flowstorm/-/blob/master/core/lib/src/commonMain/kotlin/ai/flowstorm/core/SttModel.kt) enum |
| X-SttSampleRate | sttSampleRate   | 16000                              | In Herz                                                                                                                                   |
| X-SttEncoding   | sttEncoding     | LINEAR16                           | `LINEAR16` or `MULAW`                                                                                                                     |
| X-TtsFileType   | ttsFileType     | mp3                                | `mp3` or `wav`                                                                                                                            |

### Session ID

In opposite to configuration values session ID is transported using cookie named flowstorm-session-id. V2 socket client is obliged to set or update its value received `Set-Cookie` response header and set it using `Cookie` request header. Client should discard stored session value in case of receiving ErrorEvent OR ResponseEvent with property sessionEnded set to true and sessionTimeout set to zero. If sessionTimeout is positive value, session ID should be discarded only if user does not open new conversation in specificed timeout in seconds.&#x20;

## Supported types of communication

### Plain text

```
curl -X PUT "https://core.flowstorm.ai/client?key=61f3f3270516dc6f15251ef2&deviceId=my-device" \
  -H "Content-Type: text/plain" \
  --data "What's the weather to be like in London tomorrow?"
```

&#x20;You should receive response like

```
# (audio=https://core.flowstorm.ai/file/tts/ca2dedef1082b42eafaed3b8352fbac4.mp3)
< [Joanna] It is going to be sunny in london tomorrow.
.
```

So this is example of plain text interaction which might be useful for testing purposes. Every response line represents part of response introduced by a special single character&#x20;

1. in response to **PUT** requests
   * `<` for speech text (preceeded by voice or persona name in square brackets)&#x20;
   * `#` for other response item properties and their values (`audio`, `video`, `code`, `background`) specified in parentheses, multiple separates by comma
   * `!` in case of error, in form source: text; e.g.\
     `DialogueManagerV2: Action #action1 not found in dialogue`
   * `.` if session ends&#x20;
2.  in response to **GET** requests

    * `~` for speech recognition interim result text; e.g.\
      `~ What's`\
      `~ What's the weather`\
      `~ What's the weather to be like`



{% hint style="info" %}
Don't forget that if you want to continue in conversation (in case that session has not ended) you have to keep cookie `flowstorm-session-id` in subsequent HTTP requests.
{% endhint %}

### Audio/Video input file&#x20;

In case that you have prerecorded audio input from user (e.g. you implement "push to talk" style of audio input) you can send it&#x20;



### Audio/Video input streaming

If you want to pass audio input from mic to socket directly and let Flowstorm Pipeline's streams do the recognition work on the fly, you can do it by writing to PUT request

1. Audio/Video data blocks (see list of supported [Audio/Video formats](flowstorm-socket-v2.md#audio-video-formats)). Request URL must contain query parameter `realtime=1` to get server notified that audio data are not prerecorded and therefore thay cannot be transported at once (as it is the case of [Audio/Video input file](flowstorm-socket-v2.md#audio-video-input-file) form of communication) but they are going to be send in realtime while being captured by mic/camera
2. JSON objects representing Channel events
   1. Multiple `BinaryEvent`s transporting blocks of raw PCM data (one block should contain something between 50-100ms of audio, that means thousands of bytes, depending on sample rate and format)&#x20;
   2. Optionally followed by `OutputStreamCloseEvent` to indicate that user has switched from audio to text input (no more `BinaryEvent`s should follow), followed by
   3. `InputEvent` containing user's text input

to the request body which has be transported chunked (using request header `Transfer-Encoding: chunked`) otherwise Flowstorm server app wouldn't be able to process them continuously.&#x20;

As we use standard HTTP request for input streaming, which allows by its design only half-duplex transmission, that means optional writing followed by mandatory reading in one request, in opposite to [Flowstorm Socket V1](web-socket.md) leveraging web sockets supporting full-duplex transmission, we cannot send the interim nor final result of input (speech) recognition represented by `RecognizedInputEvent` to the client in the same PUT request as client does not know when to stop audio streaming and start reading of server response. So in case of input streaming, client has to do second **GET** request running in parallel to obtain interim recognition results and, first of all, by completing GET response be notified that recognition has been finished so it has stop writing more data and the the PUT request should start reading response from it.

{% hint style="warning" %}
Don't forget to use all the same socket properties and share `flowstorm-session-id` cookie value for all **PUT** and **GET** requests in your client implementation otherwise input streaming will be ended prematurely leading to processing of `#silence` action in dialogue after timeout specified by speech recognizer's configuration (5 seconds by default).
{% endhint %}

&#x20;



&#x20;\


&#x20;
