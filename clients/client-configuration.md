# Client Configuration

## Configuration Parameters

Configuration parameters represent set of properties which can be implemented by any of platform clients.

* [Standalone application](standalone/) uses them as command line options to command `client` \(using form `--name` or `-shortcut`\)
* Mobile \([Android](android.md) and [iOS](ios.md)\) apps can have them in their settings screens
* [Web client](web.md) can use them as query parameter names

| Name | Shortcut\(s\) | Default value | Description |
| :--- | :--- | :--- | :--- |
| `environment` | `e` |  | Specific default core environment type  `local`,`preview` |
| `serverConfig` | `sc` | `false` | Allow server configuration - it will be downloaded from URL `https://admin(.$environment).promethist.com/client/deviceConfig/$sender` upon client start. Parameters loaded from server will override parameters set by client. |
| `url` | `u` |  | Custom core URL e.g. `https://core.promethist.my-company.com` |
| `key` | `k` |  | Application key containing value `appId` or `:dialogueId` |
| `secret` | `x` |  | Application secret \(optional - some may require it\) |
| `sender` | `id, s` | \(depends on  client type\) | Unique device identification |
| `language` | `l` | `en` | Preferred language |
| `autoStart` | `as` | `false` | Start conversation immediately |
| `introText` | `it` | `#intro` | Intro text or \#action |

## Configuration File

Configuration can be also stored as file in Properties or JSON format, e.g.

```text
environment=local
url=http://localhost:8080
key=5ea17702d28fd40eec1e9076
autoStart=true
```

or

```javascript
{
  "environment": "local",
  "url": "http://localhost:8080",
  "key": "5f1030bc6342ac5098f824ca",
  "autoStart": true
}
```



