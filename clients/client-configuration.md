# Client Configuration

## Configuration Parameters

Configuration parameters represent set of properties which can be implemented by any of platform clients.

* [Standalone application](standalone/) uses them as command line options to command `client` (using form `--name` or `-shortcut`)
* Mobile ([Android](android.md) and [iOS](ios.md)) apps can have them in their settings screens
* [Web client](web.md) can use them as query parameter names

| Name           | Shortcut(s) | Default value                       | Description                                                                                                                                                                                                                                          |
| -------------- | ----------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `environment`  | `e`         |                                     | <p>Specific default core environment type <br><code>local</code>,<code>preview</code></p>                                                                                                                                                            |
| `serverConfig` | `sc`        | `false`                             | <p>Allow server configuration - it will be downloaded from URL<br><code>https://admin(.$environment).flowstorm.ai/client/deviceConfig/$sender</code><br>upon client start. Parameters loaded from server will override parameters set by client.</p> |
| `url`          | `u`         |                                     | <p>Custom core URL<br>e.g. <code>https://core.flowstorm.my-company.com</code></p>                                                                                                                                                                    |
| `key`          | `k`         |                                     | Application key containing value `appId` or `:dialogueId`                                                                                                                                                                                            |
| `secret`       | `x`         |                                     | Application secret (optional - some may require it)                                                                                                                                                                                                  |
| `sender`       | `id, s`     | <p>(depends on <br>client type)</p> | Unique device identification                                                                                                                                                                                                                         |
| `language`     | `l`         | `en`                                | Preferred language                                                                                                                                                                                                                                   |
| `autoStart`    | `as`        | `false`                             | Start conversation immediately                                                                                                                                                                                                                       |
| `introText`    | `it`        | `#intro`                            | Intro text or #action                                                                                                                                                                                                                                |

## Configuration File

Configuration can be also stored as file in Properties or JSON format, e.g.

```
environment=local
url=http://localhost:8080
key=5ea17702d28fd40eec1e9076
autoStart=true
```

```javascript
{
  "environment": "local",
  "url": "http://localhost:8080",
  "key": "5f1030bc6342ac5098f824ca",
  "autoStart": true
}
```

