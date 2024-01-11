# Channel Events



<table><thead><tr><th>Name</th><th width="150">Protocol Version</th><th>Emittor</th><th>Purpose</th></tr></thead><tbody><tr><td><code>InputEvent</code></td><td>V1, V2</td><td>Client</td><td>Input object passed from client to server (e.g. when user is using text input OR ASR is being done on the client side). As soon as this event is emitted client should go to <code>Processing</code> state.</td></tr><tr><td><del><code>RequestEvent</code></del></td><td>V1 only, obsolete</td><td>Client </td><td>Transport of obsolete request object from client to the server</td></tr><tr><td><code>InputAudioStreamOpenEvent</code></td><td>V1 only</td><td>Client</td><td>Indicates beginning of the input audio stream</td></tr><tr><td><code>InputStreamCloseEvent</code></td><td>V2 only</td><td>Client</td><td>Used to indicate end of input stream when client user switches to  text input</td></tr><tr><td><code>InputAudioStreamCloseEvent</code></td><td>V1 only</td><td>Client</td><td>Indicates end of the input audio stream if client has received finally recognized input OR client user has switched to the text input</td></tr><tr><td><code>RecognizedInputEvent</code></td><td>V2 only</td><td>Server</td><td>Used to pass client interim or final input object recognized from input stream</td></tr><tr><td><code>RecognizedEvent</code></td><td>V1 only</td><td>Server</td><td>Used to pass client interim or final input text recognized from input audio stream</td></tr><tr><td><code>ResponseItemEvent</code></td><td>V1, V2</td><td>Server</td><td>Response item can contain text and/or audio, video or command code (payload) to be interpreted by the client immediately.</td></tr><tr><td><code>ResponseItem</code></td><td>V1, V2</td><td>Server</td><td>Indicates completion of the response (and optionally end of session). Client should go to and stay in the <code>Responding</code> state until it has finished all response item processing, then go to <code>Listening</code> state) or go to sleep (<code>Sleeping</code> state) depending on whether session has ended or not.</td></tr><tr><td><code>InitEvent</code></td><td>V1 only</td><td>Client</td><td>Used to initiate connection to the server</td></tr><tr><td><code>ReadyEvent</code></td><td>V1 only</td><td>Server</td><td>Acknowledge of initiated connection from the server</td></tr><tr><td><code>ErrorEvent</code></td><td>V1, V2</td><td>Server</td><td>Non-recoverable server error text and source name. Client should display or otherwise indicate (e.g. by prerecorded voice message) server error and end session instantly by going to the <code>Sleeping</code> or <code>Failed</code> state.</td></tr><tr><td><code>SessionStartEvent</code></td><td>V1 only</td><td>Server</td><td>Acknowledge of  new session start (transport of session ID; V2 socket is using cookie for that)</td></tr><tr><td><code>SessionEndEvent</code></td><td>V1 only</td><td>Server</td><td>Indication of session end (client should discard session ID; obsolete, replaced with sessionEnded property of Response object transported via <code>ResponseEvent</code>)</td></tr><tr><td><code>LogEvent</code></td><td>V1, V2</td><td>Client</td><td>Used to transport client's log entries to the server where they are stored to the current session</td></tr><tr><td><code>BinaryEvent</code></td><td>V2 only</td><td>Client<br>Server potentially in the future</td><td>Used to transport block of audio/video data from client to the server</td></tr></tbody></table>