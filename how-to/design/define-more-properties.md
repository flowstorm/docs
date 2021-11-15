# Define More Properties

Each dialogue model has certain general properties, which can be set in the "**Properties**" tab:

![](<../../.gitbook/assets/image (82).png>)

The properties include:

### **Name**

* Here, you can rename the dialogue model. The required format is `xxx/yyy` (no spaces).

### **Language**

* Set the language of the dialogue model.

### **Speech Recognizer**

* Select the speech recognizer (a.k.a. ASR provider) for the dialogue model. The default setting is Google.

### **Voice**

* Select the voice of the dialogue model. The default English voice is _Joanna (Amazon)_.
  * If this box is **empty** and the dialogue is linked to another dialogue, the voice is **inherited** from the higher-level dialogue.
* _Note: This is a general setting for the dialogue model; a different voice can be selected for any specific Speech node in the Speech node details._

### **Background**

* If you want to use a custom background image, enter here the URL of your image.
* Similar to the voice, if the box is **empty** and the dialogue is linked to another dialogue, the background will be **inherited** from a higher-level dialogue.

### **Mixins**

* Select the "**init mixins**" that you want to link to your dialogue model.
  * The init mixins can be defined in _**Design >> Mixins**_.
* Init mixins are useful when you want several dialogue models to share the same init code.
