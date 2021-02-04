# Built-In Voices

Built-in voices are predefined in [`org.promethist.core.model.Voice`](https://github.com/PromethistAI/core/blob/master/lib/src/main/kotlin/org/promethist/core/model/Voice.kt) enumeration. They can set to dialogue models by `voice` property instead of specifying exact TTS configuration in property `ttsConfig` \(older versions of platform supported only built-in voices\). If core [runner](../core-services/runner.md) service is providing speech response to IVR platform from different provider than voice provider specified for dialogue or particular speech \(e.g. Amazon voice specified in dialogue running on Google Home\), it will substitute it by default voice of target provider for the same language / locale and gender. 

| Name | Locale | Gender | Default For | Provider Name | Provider Voice | Provider Engine |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Audrey | en\_US | Female | en / en\_US | Amazon | Joanna | neural |
| Anthony | en\_US | Male | en / en\_US | Amazon | Matthew | neural |
| Grace | en\_US | Female |  | Google | en-US-Standard-C |  |
| George | en\_US | Male |  | Google | en-US-Standard-B |  |
| Mary | en\_GB | Female |  | Microsoft | en-US-AriaNeural |  |
| Michael | en\_GB | Male |  | Microsoft | en-US-GuyNeural |  |
| Amy | en\_GB | Female | en\_GB | Amazon | Amy | neural |
| Arthur | en\_GB | Male | en\_GB | Amazon | Brian | neural |
| Gwyneth | en\_GB | Female |  | Google | en-GB-Wavenet-C |  |
| Gordon | en\_GB | Male |  | Google | en-GB-Wavenet-B |  |
| Gabriela | cs\_CZ | Female | cs | Google | cs-CZ-Standard-A |  |
| Milan | cs\_CZ | Male | cs | Microsoft | cs-CZ-Jakub |  |

