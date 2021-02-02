# Client Integration

Client integration is based on [web socket](../core/client-integrations/web-socket.md) provided by Core runner service.

## Java/Android Client

If you want to create your own client in Java or Android platforms, you can use **Promethist Common Client library** for that. The whole source code of it is a part of Promethist Core project and can be found on [GitHub](https://github.com/PromethistAI/core/tree/master/client). 

{% hint style="warning" %}
We recommend using Java version 8 \(Oracle or OpenJDK\). Newer version can have certain problems with specific functionalities \(e.g. screen visualisations based on JavaFX technology\) therefore we do not officially support them yet. 
{% endhint %}

First, add PromethistAI artifact repository to your project

{% tabs %}
{% tab title="Maven" %}
```markup
<repositories>
    <repository>
        <id>promethistai</id>
        <name>PromethistAI repository</name>
        <url>https://repository.promethist.ai/maven/</url>
    </repository>
</repositories>
```
{% endtab %}

{% tab title="Gradle" %}
```groovy
allprojects {
    repositories {
        maven { url 'https://repository.promethist.ai/maven/' }
    }
}
```
{% endtab %}
{% endtabs %}

and set dependency to `promethist-client-common`  to your project or module

{% tabs %}
{% tab title="Maven" %}
```markup
<dependency>
    <groupId>org.promethist.client</groupId>
    <artifactId>promethist-client-common</artifactId>
    <version>2.19</version>
</dependency>
```
{% endtab %}

{% tab title="Gradle" %}
```groovy
dependencies {
    implementation 'org.promethist.client:promethist-client-common:2.19'
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Latest stable version number can be checked [here](https://github.com/PromethistAI/core/tags)
{% endhint %}

### Callback Object

First of all, every client has to implement [`BotClientCallback`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/BotClientCallback.kt) interface - following example demonstrates it \(you can replace method implementations just printing what's has occurred with audio/visual processing logic of your client\)

```kotlin
class Callback : BotClientCallback {

    override fun onOpen(client: BotClient) = println("{Open}")
    override fun onReady(client: BotClient) = println("{Ready}")
    override fun onClose(client: BotClient) = println("{Closing}")
    override fun onError(client: BotClient, text: String) = println("{Error $text}")
    override fun onFailure(client: BotClient, t: Throwable) = t.printStackTrace()
    override fun audioCancel() = println("{Audio Cancel}")
    override fun command(client: BotClient, command: String, code: String?) = println("{Command $command $code}")
    override fun httpRequest(client: BotClient, url: String, request: HttpRequest?) = HttpUtil.httpRequest(url, request)

    override fun onSessionId(client: BotClient, sessionId: String?) = println("{Session ID $sessionId}")
    override fun onBotStateChange(client: BotClient, newState: BotClient.State) = println("{State change to $newState}")
    override fun onWakeWord(client: BotClient) = println("{Wake word}")

    override fun audio(client: BotClient, data: ByteArray) {
        println("{Audio ${data.size} bytes}")
        //NOTE: this method should implement audio playing queue and state should 
        // be changed to listening when the last audio was played and queue is empty
        // see Voice Bot article for more information regarding output audio
        if (client.state == BotClient.State.Responding)
            client.state = BotClient.State.Listening
    }
    override fun video(client: BotClient, url: String) = println("{Video $url}")
    override fun image(client: BotClient, url: String) = println("{image $url}")
    override fun text(client: BotClient, text: String) = println("< $text")

    override fun onRecognized(client: BotClient, text: String) = println("> $text")
    override fun onLog(client: BotClient, logs: MutableList<LogEntry>) = logs.forEach { println(it) }
}
```

Then, you need to create [`BotContext`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/BotContext.kt) object defining relation between client a conversational application

```kotlin
val context = BotContext(
    "https://core.promethist.com", // Core service URL
    "5ea17702d28fd40eec1e9076", // application ID
    "device_ID", // CHANGE TO unique sender device identification
    Dynamic("clientType" to "client_type:1.0.0-SNAPSHOT"), // CHANGE TO your-client-name:version
    autoStart = false // set to true if you want to start application immediately as soon as client is open
)
```

### Bot Client 

Client is represented by [`BotClient`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/BotClient.kt) class which implements network communication with the Core service using web socket, processing input audio and playing output audio. To understand better networking part of implementation please take a look on [web socket](../core/client-integrations/web-socket.md) page too.

#### State

It is always in one of following states accessible via public property `state`

| State | Description |
| :--- | :--- |
| `Listening` | Waiting for user input \(if input audio device is available as described below, sending input audio to the server; otherwise waiting for text input by calling `doText` method\) |
| `Processing` | User text input send or server recognized text in input audio, waiting for response |
| `Responding` | Processing server response = display response text\(s\)/images and playing audio\(s\). As soon as the last audio play is finished it will go back to `Listening` state IF the response have not finished session \(in that case client goes immediately to `Sleeping` state\) |
| `Open` | Short time state between opening web socket and obtaining response to Init event. |
| `Closed` | Network connection is closed \(after calling `close` method\)  |
| `Failed` | Network connection has failed. Client will keep trying to restore it every 20 seconds |

#### Methods

<table>
  <thead>
    <tr>
      <th style="text-align:left">Method</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>open()</code>
      </td>
      <td style="text-align:left">Open communication to the server. Call this method first and once before
        any other</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>doText(text: String, attributes: PropertyMap = emptyMap())</code>
      </td>
      <td style="text-align:left">Send text input to the server e.g. <code>#signal</code>, <code>what&apos;s the weather</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>doIntro()</code>
      </td>
      <td style="text-align:left">Initiate conversation - equal to call <code>toText(&quot;#intro&quot;)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>touch(openInputAudio: Boolean = true)</code>
      </td>
      <td style="text-align:left">
        <p>Touch client (usually - client calls it after user clicks or touches &quot;play&quot;
          button). Depending on client state</p>
        <ul>
          <li><code>Sleeping</code> - cancel audio playing, calls <code>doIntro() </code>
          </li>
          <li><code>Responding</code> - if client has pausing enabled, it will pause;
            otherwise output audio will be canceled and client will go to <code>Listening</code>
          </li>
          <li><code>Paused</code> - resume output audio</li>
          <li><code>Closed</code> or <code>Failed</code> - error audio response</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>close()</code>
      </td>
      <td style="text-align:left">Close client. Output audio play is cancelled, input audio device and network
        socked are close.</td>
    </tr>
  </tbody>
</table>

### Text Bot 

If you want to create simple text bot, without audio support, you can create [`BotClient`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/BotClient.kt) without [`AudioDevice`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/audio/AudioDevice.kt) object

```kotlin
val audioDevice: AudioDevice? = null
val client = BotClient(context, OkHttp3BotClientSocket(context.url), audioDevice, Callback())
client.open()
```

 You can create simple testing text client working with standard input/output using [`TextConsole`](https://github.com/PromethistAI/core/blob/master/common/lib/src/main/kotlin/org/promethist/common/TextConsole.kt) object

```kotlin
object : TextConsole() {
    override fun afterInput(text: String) = client.doText(text)
}.run() 
```

###  Voice Bot

If you want to create voice \(input audio\) using client, you have to create object of  [`AudioDevice`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/audio/AudioDevice.kt) class and pass it to [`BotClient`](https://github.com/PromethistAI/core/blob/master/client/lib/src/main/kotlin/org/promethist/client/BotClient.kt)  constructor. When using Java JRE, you can take [`Microphone`](https://github.com/PromethistAI/core/blob/master/client/standalone/src/main/kotlin/org/promethist/client/standalone/io/Microphone.kt) class from [Standalone application](../clients/standalone/) using Java Sound API.

```kotlin
val audioDevice = Microphone()

// if your microphone has multiple channels, you have to specify 
// count and index of microphone channel to be used
val audioDevice = Microphone(channels = 6, channel = 1)
```

{% hint style="info" %}
Microphone sample rate is set to 16000 Hz, size 16 bits, signed, little endian by default.  
{% endhint %}

#### Custom Input Audio Device

If case that you have specific device providing audio input you can implement your own [`InputAudioDevice`](https://github.com/PromethistAI/core/blob/master/client/common/src/main/kotlin/org/promethist/client/audio/InputAudioDevice.kt)

```kotlin
class MyAudioDevice : InputAudioDevice() {
    override fun read(buffer: ByteArray): Int = TODO("Add audio data reading code here")
}
```

Example of implementation for Android devices

```kotlin
class AudioRecorder(var autoClose: Boolean = false) : InputAudioDevice() {

    companion object {

        private val SAMPLE_RATE_CANDIDATES = intArrayOf(16000, 11025, 22050, 44100)
        private val CHANNEL = AudioFormat.CHANNEL_IN_MONO
        private val ENCODING = AudioFormat.ENCODING_PCM_16BIT
    }

    var bufferSize: Int = 0
    var audioRecord: AudioRecord? = null

    override fun start() {
        audioRecord = createAudioRecord()
        audioRecord!!.startRecording()
        if (audioRecord?.recordingState == AudioRecord.RECORDSTATE_RECORDING)
            super.start()
    }

    override fun stop() {
        super.stop()
        if (audioRecord?.state == AudioRecord.RECORDSTATE_RECORDING)
            audioRecord?.stop()
        audioRecord?.release()
        audioRecord = null
    }

    override fun read(buffer: ByteArray): Int = audioRecord!!.read(buffer, 0, buffer.size)

    private fun createAudioRecord(): AudioRecord {
        for (sampleRate in SAMPLE_RATE_CANDIDATES) {
            var sizeInBytes = AudioRecord.getMinBufferSize(sampleRate, CHANNEL, ENCODING)
            if (sizeInBytes == AudioRecord.ERROR_BAD_VALUE) {
                continue
            }
            sizeInBytes = sizeInBytes * 4
            val audioRecord = AudioRecord(
                MediaRecorder.AudioSource.MIC,
                sampleRate,
                CHANNEL,
                ENCODING, sizeInBytes
            )
            if (audioRecord.state == AudioRecord.STATE_INITIALIZED) {
                bufferSize = sizeInBytes
                return audioRecord
            } else {
                audioRecord.release()
            }
        }
        throw RuntimeException("Cannot instantiate AudioRecorder")
    }
}
```

