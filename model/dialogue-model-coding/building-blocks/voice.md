# Voices



## How Voices work on Amazon Alexa and Google Assistant

If your dialogue is running on Amazon Alexa or Google Assistant then speech synthesis \(TTS\) is performed on their side instead of in the platform Core[ runner](../../../core/core-services/runner.md). As they support only particular voice settings using SSML tag `voice`  we must do some some mapping logic to add this tag into the output and make speech sound as similar as possible to the voice TTS configuration. 

### Amazon Alexa

a\) If applied voice TTS configuration has **Amazon Alexa** property set, it will result into SSML 

```kotlin
<voice name="${ttsConfig.amazonAlexa}">$ssml</voice>
```

{% hint style="info" %}
Set Amazon Alexa property on voices using different TTS provider then Amazon or if Amazon \(Amazon Polly service\) voice name is not supported by Alexa.
{% endhint %}

b\) If applied voice TTS configuration has **Provider** set to **Amazon** value, it will use **Name** property value for SSML 

```kotlin
<voice name="${ttsConfig.name}">$ssml</voice>
```

{% hint style="warning" %}
Please ensure that TSS configuration voice name is supported by Amazon Alexa - if not, set appropriate voice name explicitly using **Amazon Alexa** property \(see a\) block\).  
{% endhint %}

c\) otherwise no `<voice>` tag will be applied to your speech \(default Alexa voice will be used\)

{% hint style="info" %}
To see list of voice names supported by Alexa please take a look on documentation page [here](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html#voice).
{% endhint %}

### Google Assistant

Applied voice TTS configuration **Gender** property will be used together with optionally set **Google Assistant** property value \(if it is empty, value `1` will be used instead\) used as as assistant voice variant

```kotlin
<voice gender="${ttsConfig.gender}" variant="${ttsConfig.googleAssistant ?: 1}">$ssml</voice> 
```

