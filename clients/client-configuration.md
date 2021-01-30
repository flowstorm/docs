# Client Configuration

## Configuration Parameters

| Name | Shortcut | Default value | Description |
| :--- | :--- | :--- | :--- |
| `serverConfig` | `sc` | `false` | Allow server configuration |
| `environment` | `e` |  | Specific default core environment type  `develop`,`preview` |
| `url` | `u` |  | Custom core URL e.g. https://core.promethist.company.com |
| `key` | `k` |  | Application key containing value `appId` or `:dialogueId` |
| `secret` | `x` |  | Application secret \(optional\) |
| `sender` | `s` | depends on  client type | Unique device identification |
| `language` | `l` | `en` | Preferred language |
| `autoStart` | `as` | `false` | Start conversation immediately |
| `introText` | `it` | `#intro` | Intro text or \#action |
| `exitOnError` | `ex` | `false` | Exit client on exception |
| `noOutputLogs` | `nol` | `false` | No output logs \(e.g. `{Ready}`, `{Sleeping > Responding}` etc.\) |
| `showLogs` | `log` | `false` | Show [contextual logs](../programming/dialoguescript/logging.md) |
| `noCache` | `nc` | `false` | Do not cache anything \(audio and image files\) |

## Configuration File





