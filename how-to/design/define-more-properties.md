# Define more properties

Each dialogue has certain general properties, which can be set in the "**Properties**" tab:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

The properties include:

### **Name**

* Here, you can rename the dialogue. The required format is `xxx/yyy` (no spaces or special characters).

### **Language**

* Set the language of the dialogue.

### **Persona**

* Select the persona who will handle the dialogue.
* If this box is **empty** and the dialogue is linked to another dialogue, the voice is **inherited** from the higher-level dialogue.

### **Voice**

* If you want to change the selected persona's voice, choose it here. The default English-speaking voice is _Joanna (Amazon)_.
  * If this box is **empty** and the dialogue is linked to another dialogue, the voice is **inherited** from the higher-level dialogue.
* _Note: This is a general setting for the dialogue; a different voice can be selected for any specific Speech node in Speech node Properties._

### **Background**

* If you want to use a custom background image, enter the URL of your image here.
* As with the persona/voice setting, if the box is left **empty** and the dialogue is linked to another dialogue, the background will be **inherited** from the higher-level dialogue.

### **Speech Recognizer**

* Select the speech recognizer (a.k.a. ASR provider) for the dialogue. The default setting is Google.

### **Mixins**

* Select the "**init mixins**" that you want to link to your dialogue model.
  * The init mixins can be defined in _**Design >> Mixins**_.
* Init mixins are useful when you want several dialogue models to share the same init code.
