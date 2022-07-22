---
description: Documentation for the Flowstorm Android library
---

# Android library

The Flowstorm Android library was create to allow easy creation of voice-enabled applications or adding a support for the Flowstorm communication into custom applications. This page describes how to import and use the library.

### Pre-requisites

The library uses the [Koin library](https://insert-koin.io/) for module injection. Add the dependency to your `build.gradle.kts` file using

```kts
implementation("io.insert-koin:koin-android:3.2.0")
```

The library itself is imported using

```kts
implementation("ai.flowstorm.client.shared:flowstorm-client-shared-android:1.0.0"){
    isTransitive = true
}
```

The attribute `isTransitive` is required, because the library is distributed in the AAR format which does not include the dependencies the library requires.

#### Speech recognition

Note that even though you don't need to define it yourself in your `AndroidManifest`, the app will require the microphone access rights from the user. Also, if the user has the "Google" application disabled on their phone, the ASR won't work.

### Importing the library

In your application's `MainApplication` file, add the following to initialize the library:

```kotlin
import ai.flowstorm.android.lib.Preferences
import ai.flowstorm.android.util.FlowstormInitializer

val appModule = FlowstormInitializer.initializeApp(this).apply { }

startKoin {
    androidLogger()
    androidContext(this@MainApplication)
    modules(appModule)
}

val preferences: Preferences = getKoin().get()
preferences.setupFromDefaultConfig()
```

If you have your own Koin modules, you can add them in the `apply` block on line 1.

### Running an activity from the library

This example shows how to start a conversation activity in chat mode, with default settings. For more info about available activities, see the [Library contents](android-library.md#undefined) section.  In your Activity, simply add

```
val intent = Intent(this, ChatActivity::class.java)
startActivity(intent)
overridePendingTransition(android.R.anim.fade_in, android.R.anim.fade_out)
```

Upon reaching this block of code, the conversation with Flowstorm will start.

### Configuring the client

The `initializeApp()` function takes an argument `customDefaults` of type `FlowstormDefaults`. By creating your own class extending that one, you can configure the client for your needs. This is the structure of the class, along with the default parameters:

```
open class FlowstormDefaults: Defaults {
    override val appKey = "624c00370cf9df6d5508c088"
    override val language = "en_US"
    override val coreHost = "core.flowstorm.ai"
    override val personaListUrl: String = "https://repository.flowstorm.ai/status/personas.json"
    override val name = "flowstorm"
    override val preferredInteractionModel: String = Defaults.PreferredInteractionModel.VOICE.name
    
    override val autoStart = true
    override val clientVersion: String = "2"
    override val sentryForceLogging: Boolean = false
    override val debugMode: Boolean = false
    
    override val enableAvatar: Boolean = false
    override val streamHost = "4dbc-147-32-71-6.ngrok.io"
    override val iceServerUri = "stun:stun.l.google.com:19302"
}
```

* **appKey** (String) - The key to your conversational application. It can be obtained in the Flowstorm Studio
* **language** (String) - The language which will be used in the speech recognition
* **coreHost** (String) - The URL of the Flowstorm backend
* **personaListUrl** (String) - The URL for your persona configuration. For the application to display a picture and a name of a persona it needs this configuration file where the mapping from voice to persona is defined. If you use custom voices, you can download the file from the default URL, extend it with your own personas, upload it elsewhere and link it here.
* **name** (String) - this will be used in the `clientType` field during communication
* **preferredInteractionModel** (PreferredInteractionModel) - If the conversation is launched through MenuActivity, but the dialogue does not start with the `#modules` command, this will be the interaction mode used.
* **autoStart** (Boolean) - sets whether to start the conversation automatically by sending `#intro` input
* **clientVersion** (String) - whether to communicate using socket (1) or using HTTP (2)
* **sentryForceLogging** (Boolean) - if `true`, the Sentry module (if enabled) will log all events. If false, only 10% of events will be logged. More about Sentry in the [Logging](android-library.md#logging) section.
* **debugMode** (Boolean) - Enables certain UI elements
* **enableAvatar** (Boolean) - If `true`, starts the application with the 3D avatar stream enabled
* **streamHost** (String) - The URL of the avatar stream
* **iceServerUri** (String) - The URL of the STUN server

The most important/useful parameters are probably `appKey`, `language` and `name`.

### Library contents

The main feature of the library are the two conversational and three other activities/screens. The activities are self-sufficient, the library contains everything to make them work - dependency definitions, user permissions, layouts (and other resources) and imported classes.

![Menu activity](../.gitbook/assets/Screenshot\_20220722-134607.png) ![Voice activity](<../.gitbook/assets/Screenshot\_20220722-134626 (1).png>) ![Chat activity](<../.gitbook/assets/Screenshot\_20220722-134638 (1).png>)

#### Voice activity

The first conversation screen, implementing the "voice mode". The layout is designed to resemble a phone call. The red button ends the conversation. The speaker button turns off and on audio output. The bubble button turns on and off subtitles, both for the persona and for the user. The microphone icon will flash green when the user is supposed to speak, the persona image will flash white when the persona speaks.

#### Chat activity

The other conversation screen, implementing the "chat mode". The layout is designed to resemble an instant messaging application. The messages from the persona will appear on the left. The user can type their message using the input field, then send them.

#### Menu activity

A screen that displays a selection of personas. Swiping the upper part changes between personas, swiping the lower part changes between topics the persona knows (if there is more than one). Clicking the phone receiver starts the voice mode, the bubble activity starts the chat mode. The user can also make the persona introduce themselves by clicking the video. The buttons in the upper part of the screen lead to settings (left) and profile (right).

#### Profile activity

A screen where the info about the user is displayed.

#### Settings activity

A screen where the user can configure the app.

#### Other classes

Each class of the library can of course be imported on its own. To name one example, the importing application can use the Android ASR in the class `FlowstormSpeechRecognizer`. To initialize it, you need to pass a `RecognitionListener` - for that, you can use the `FlowstormRecognitionListener`, or extend it. After that, call the `createSpeechRecognizer()` function. The function returns a boolean value which indicates whether the creation was successful. Afterwards, call `startListening()` to begin the ASR. You can also interrupt it by calling `stopListening()`.

### Logging

The library supports two methods of logging: Firebase and Sentry.

#### Firebase

To make the library use Firebase, you need to enable the Firebase support in your application. That is described [here](https://firebase.google.com/docs/android/setup). Then, you need to pass a `FirebaseAnalytics` object to the `initializeApp()` function.&#x20;

```kotlin
import com.google.firebase.FirebaseApp
import com.google.firebase.analytics.ktx.analytics
import com.google.firebase.ktx.Firebase


class MainApplication : Application() {

    override fun onCreate() {
        super.onCreate()
        // ...
        FirebaseApp.initializeApp(this)
        val firebaseAnalytics = Firebase.analytics
        val appModule = FlowstormInitializer.initializeApp(this, analytics = firebaseAnalytics).apply { }
    }
}
```

You also need to provide your own `google-services.json` file.

#### Sentry

To enable Sentry logging, add the following lines to the `AndroidManifest.xml` file under the `<application>` tag:

```xml
<meta-data android:name="io.sentry.dsn" android:value="YOUR_SENTRY_DSN_KEY" />
<meta-data android:name="io.sentry.traces.sample-rate" android:value="1.0" />
<meta-data android:name="io.sentry.sample-rate" android:value="1.0" />
```

### Styling

The library allows changing strings, themes, colors, images, even layouts. Any resource can be overwritten by a file or element of the same name in your application. For example, to change the default loading animation, you need to provide a drawable file named `loading_spin_anim.xml`. You can check the original names in the library files.

#### Themes

The default theme for the library is called `Theme.Flowstorm`. You can inherit from it and change certain colors.

```xml
<style name="Theme.Custom" parent="Theme.Flowstorm">
        <item name="colorPrimary">@color/white</item>
</style>
```

Then, you need to replace the theme in your `AndroidManifest.xml` file:

```xml
<application
...
tools:replace="android:theme"
android:theme="Theme.Custom"
>
```

#### Colors

```xml
<resources>
    <color name="sand_basic">#FF018786</color>
</resources>
```

#### Strings

```xml
<resources>
    <string name="settings_language">Speech recognition language</string>
</resources>
```

#### Layouts

In theory, you can change even the layouts of the activities, if you provide a file named e.g. `voice_activity.xml`. However, your layout would need to contain elements with all ids which are present in the library layout, else the application would crash after trying to access UI elements whose elements would be missing.
