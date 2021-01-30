# Command Line Interface

## Command Syntax

`java -jar promethist.jar <command> (options)`

{% hint style="info" %}
If you are using Linux or Mac you can create executable script `/usr/local/bin/promethist`   
`#!/bin/bash  
java -jar ~/promethist.jar $@`

Following command line examples will use simple command `promethist`.
{% endhint %}

## Available Commands

| Name | Description |
| :--- | :--- |
| `help` | Display commands and their usage |
| `version` | Show application version |
| `client` | Conversation client \(see available options below\) |
| `call` | Outbound call via Twilio \(see available options below\) |
| `tool` |  Development tools \(see available options below\) |

### `client`

Client command support full range of [client configuration](../client-configuration.md) parameters. Every parameter can be used as an option using shortcut \(e.g. `-sc`\) or name \(e.g. `--serverConfig`\). Above this basic set of options there are other specific for Standalone application described in following table.

#### Options

| Option\(s\) | Default value | Description |
| :--- | :--- | :--- |
| `-c, --config` |  | Config file \(contained values will override options passed in command line\) |
| `-d, --device` | `desktop` | Device type `desktop`, `model1`, `model2`, `model3` |
| `-ex, --exitOnError` | `false` | Exit client on exception |
| `-nol, --noOutputLogs` | `false` | No output logs \(e.g. `{Ready}`, `{Sleeping > Responding}` etc.\) |
| `-log, --showLogs` | `false` | Show [contextual logs](../../programming/dialoguescript/logging.md) |
| `-nc, --noCache` | `false` | Do not cache anything \(audio and image files\) |
| **Audio related** |  |  |
| `-nia, --noInputAudio` | `false` | No input audio \(text input only\) |
| `-noa, --noOutputAudio` | `false` | No output audio \(text input only\) |
| `-pm, --pauseMode` | `false` | Pause mode \(wake word or button will pause output audio instead of stopping it and listening\) |
| `-aru, --audioRecordUpload` | `none` | Audio record with upload mode  `none`, `local`, `night`, `immediate` |
| `-stt, --sttMode` | `SingleUtterance` | Speech-To-Text mode  `Default`, `SingleUtterance`, `Duplex` |
| `-sd, --speechDevice` | `none` | Specific speech device implementation, providing extra data related to speech `none`, `respeaker2` |
| `-mc, --micChannel` | `1:0` | Microphone channels \(count:selected-index\) |
| `-spk, --speakerName` |  | Speaker name |
| **Screen related** |  |  |
| `-scr, --screen` | `none` | Screen view `none`, `window`, `fullscreen` |
| `-nan, --noAnimations` | `false` | No screen view animations |
| **Network related** |  |  |
| `-sp, --socketPing` | `10` | [Web Socket](../../core/client-integrations/web-socket.md) keep-alive ping period in seconds |
| `-st, --socketType` | `OkHttp3` | Socket implementation type `OkHttp3`, `JWS` |
| `-aa, --autoUpdate` | `false` | Auto update |
| `-du, --distUrl` | [https://repository.promethist.ai/dist](https://repository.promethist.ai/dist) | Distribution URL for auto updates |

#### Examples

```bash
# start conversation with any application available for me, immediately
promethist client -as

# conversation with specific application immediately
promethist client -k 5ea17702d28fd40eec1e9076 -as

# without input and output audio
promethist client -k 5ea17702d28fd40eec1e9076 -as -nia -noa

# using specific core
promethist client -u https://core.promethist.server.com

# with fullscreen projection
promethist client -scr fullscreen
```

### `call`

| Option\(s\) | Default value | Description |
| :--- | :--- | :--- |
| `-u, --url` |  | Custom Core URL |
| `-a, --account` |  | Twilio Account SID |
| `-t, --token` |  | Twilio Auth Token |
| `-f, --from` |  | Call from number |
| `-o, --to` |  | Call to number |
| `-k, --key` |  | Application key |
| `-l, --language` |  | Preferred language |

### `tool`

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option(s)</th>
      <th style="text-align:left">Default value</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>-a, --action</code>
      </td>
      <td style="text-align:left">audio</td>
      <td style="text-align:left">
        <p>Action</p>
        <p><code>play</code>, <code>sample</code>, <code>audio</code>, <code>respeaker2</code>, <code>nmea</code>, <code>signal</code>, <code>props</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

```bash
promethist tool -a play -i test.mp3
```

