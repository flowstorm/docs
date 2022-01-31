# Command Line Interface

## Command Syntax

`java -jar flowstorm.jar <command> (options)`

{% hint style="info" %}
If you are using Linux or Mac you can create executable script `/usr/local/bin/flowstorm` \
`#!/bin/bash`\
`java -jar ~/flowstorm.jar $@`

Following command line examples will use simple command `flowstorm`.
{% endhint %}

## Available Commands

| Name      | Description                                            |
| --------- | ------------------------------------------------------ |
| `help`    | Display commands and their usage                       |
| `version` | Show application version                               |
| `client`  | Conversation client (see available options below)      |
| `call`    | Outbound call via Twilio (see available options below) |
| `tool`    |  Development tools (see available options below)       |

### `client`

Client command support full range of [client configuration](../client-configuration.md) parameters. Every parameter can be used as an option using shortcut (e.g. `-sc`) or name (e.g. `--serverConfig`). Above this basic set of options there are other specific for Standalone application described in following table.

#### Options

| Option(s)                   | Default value                                                                  | Description                                                                                                                |
| --------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config`              |                                                                                | Config file (contained values will override options passed in command line)                                                |
| `-d, --device`              | `desktop`                                                                      | <p>Device type<br><code>desktop</code>, <code>model1</code>, <code>model2</code>, <code>model3</code></p>                  |
| `-ex, --exitOnError`        | `false`                                                                        | Exit client on exception                                                                                                   |
| `-nol, --noOutputLogs`      | `false`                                                                        | No output logs (e.g. `{Ready}`, `{Sleeping > Responding}` etc.)                                                            |
| `-log, --showLogs`          | `false`                                                                        | Show [contextual logs](../../development/dialoguescript/logging.md)                                                        |
| `-nc, --noCache`            | `false`                                                                        | Do not cache anything (audio and image files)                                                                              |
| **Audio related**           |                                                                                |                                                                                                                            |
| `-nia, --noInputAudio`      | `false`                                                                        | No input audio (text input only)                                                                                           |
| `-noa, --noOutputAudio`     | `false`                                                                        | No output audio (text input only)                                                                                          |
| `-pm, --pauseMode`          | `false`                                                                        | Pause mode (wake word or button will pause output audio instead of stopping it and listening)                              |
| `-aru, --audioRecordUpload` | `none`                                                                         | <p>Audio record with upload mode <br><code>none</code>, <code>local</code>, <code>night</code>, <code>immediate</code></p> |
| `-stt, --sttMode`           | `SingleUtterance`                                                              | <p>Speech-To-Text mode <br><code>Default</code>, <code>SingleUtterance</code>, <code>Duplex</code></p>                     |
| `-sd, --speechDevice`       | `none`                                                                         | Specific speech device implementation, providing extra data related to speech `none`, `respeaker2`                         |
| `-mc, --micChannel`         | `1:0`                                                                          | Microphone channels (count:selected-index)                                                                                 |
| `-spk, --speakerName`       |                                                                                | Speaker name                                                                                                               |
| **Screen related**          |                                                                                |                                                                                                                            |
| `-scr, --screen`            | `none`                                                                         | <p>Screen view<br><code>none</code>, <code>window</code>, <code>fullscreen</code></p>                                      |
| `-nan, --noAnimations`      | `false`                                                                        | No screen view animations                                                                                                  |
| **Network related**         |                                                                                |                                                                                                                            |
| `-sp, --socketPing`         | `10`                                                                           | [Web Socket](../../project/client-integrations/flowstorm-sockets/web-socket.md) keep-alive ping period in seconds          |
| `-st, --socketType`         | `OkHttp3`                                                                      | <p>Socket implementation type<br><code>OkHttp3</code>, <code>JWS</code></p>                                                |
| `-aa, --autoUpdate`         | `false`                                                                        | Auto update                                                                                                                |
| `-du, --distUrl`            | [https://repository.promethist.ai/dist](https://repository.promethist.ai/dist) | Distribution URL for auto updates                                                                                          |

#### Examples

```bash
# start conversation with any application available for me, immediately
flowstorm client -as

# conversation with specific application immediately
flowstorm client -k 5ea17702d28fd40eec1e9076 -as

# without input and output audio
flowstorm client -k 5ea17702d28fd40eec1e9076 -as -nia -noa

# using specific core
flowstorm client -u https://core.promethist.server.com

# with fullscreen projection
flowstorm client -scr fullscreen
```

### `call`

| Option(s)        | Description        |
| ---------------- | ------------------ |
| `-u, --url`      | Custom Core URL    |
| `-a, --account`  | Twilio Account SID |
| `-t, --token`    | Twilio Auth Token  |
| `-f, --from`     | Call from number   |
| `-o, --to`       | Call to number     |
| `-k, --key`      | Application key    |
| `-l, --language` | Preferred language |

### `tool`

| Option(s)      | Default value | Description                                                                                                                                                         |
| -------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-a, --action` | `audio`       | <p>Action</p><p><code>play</code>, <code>sample</code>, <code>audio</code>, <code>respeaker2</code>, <code>nmea</code>, <code>signal</code>, <code>props</code></p> |
|                |               |                                                                                                                                                                     |

```bash
# play MP3 file
flowstorm tool -a play -i test.mp3

# list audio devices
flowstorm tool -a audio

# list java properties
flowstorm tool -a props

# respeaker2 test, showing speech detection and angle
flowstorm tool -a respeaker2

# test signal processing
flowstorm -l INFO tool -a signal -i flowstorm.json

# test reading of NMEA data from local file
flowstorm tool -a nmea -i /dev/path-to-nmea-input

# test reading of NMEA data from network socket
flowstorm tool -a nmea -i 10.0.1.45:11123
```
