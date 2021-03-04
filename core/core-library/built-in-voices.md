# Built-In Voices

Built-in voices are predefined in [`ai.flowstorm.core.model.Voice`](https://github.com/PromethistAI/core/blob/master/lib/src/main/kotlin/org/promethist/core/model/Voice.kt) enumeration. They can be set to dialogue model `voice` property instead of specifying exact TTS configuration in property `ttsConfig` \(older versions of platform supported only built-in voices\). If core [runner](../core-services/runner.md) service is providing speech response to IVR platform from different provider than voice provider specified for dialogue or particular speech \(e.g. Amazon voice specified in dialogue running on Google Home\), it will substitute it by built-in voice of target provider for the same language / locale and gender \(see list of configurations with market defaults below\)

## Built-in Voice Configurations

| Name | Locale | Gender | Provider Name | Provider Voice | Provider Engine |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Audrey** | en\_US | Female | Amazon | Joanna | neural |
| **Anthony** | en\_US | Male | Amazon | Matthew | neural |
| **Grace** | en\_US | Female | Google | en-US-Standard-C |  |
| **George** | en\_US | Male | Google | en-US-Standard-B |  |
| **Mary** | en\_GB | Female | Microsoft | en-US-AriaNeural |  |
| **Michael** | en\_GB | Male | Microsoft | en-US-GuyNeural |  |
| **Amy** | en\_GB | Female | Amazon | Amy | neural |
| **Arthur** | en\_GB | Male | Amazon | Brian | neural |
| **Gwyneth** | en\_GB | Female | Google | en-GB-Wavenet-C |  |
| **Gordon** | en\_GB | Male | Google | en-GB-Wavenet-B |  |
| **Gabriela** | cs\_CZ | Female | Google | cs-CZ-Standard-A |  |
| **Milan** | cs\_CZ | Male | Microsoft | cs-CZ-Jakub |  |

