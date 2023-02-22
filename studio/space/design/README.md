---
description: Tools for designing different types of assets.
---

# Design

## [Dialogues](https://app.flowstorm.ai/#!/space/dialogue)

The **main editing environment** **for** **conversation design**. This is where you create, test, and modify your dialogues – the node graphs as well as the related code.

Read [this article](dialogue-designer.md) for more details.

## [Entities](https://app.flowstorm.ai/#!/space/datasets)

Create, train, and modify **entities** and **entity datasets**.

An entity refers to a specific object, concept, or element in a text that is recognized and classified based on its relevance to a particular domain or task. Entities can be anything from people, locations, and organizations to dates, times, and numerical values.

More about entities [here](https://docs.flowstorm.ai/development/dialoguescript/user-input/entities).

## [Snippets](https://app.flowstorm.ai/#!/space/snippets)

Create and modify reusable dialogue graph snippets. Snippets can save you time as you won't need to create the same structure again and again – you can just easily drag\&drop a copy of a pre-made snippet into any dialogue.

Read [this article](snippet-designer.md) for more details.

## [Mixins](https://app.flowstorm.ai/#!/space/mixins)

Create and modify dialogue mixins. These are **referenceable** contents of different types of nodes (Intent, Speech, Command, Function, User Input, or the "Init" code of a dialogue).

You can link a mixin to a specific node, and any updates made to the mixin content will automatically update the node (after the next dialogue build).

Read [this article](dialogue-mixin.md) for more details.

## [Files](https://app.flowstorm.ai/#!/space/assets)

Upload file assets (**images, sounds, JSON files**) that you will be able to reference and use in your dialogues.

In case you need a direct URL of a file asset, you will find it in the Properties of the file in question.

## [Personas](https://app.flowstorm.ai/#!/space/personas)

Design digital personas that will handle your dialogues. A persona is defined by a combination of certain features like voice, gender, or the **implicit dialogue** containing persona-specific reactions to relevant global intents.

## [Voices](https://app.flowstorm.ai/#!/space/voices)

Reference existing TTS (text-to-speech) voices from external providers. These voices can then be selected in [Personas](./#personas) or [Dialogues](./#dialogues).

Currently supported providers: **Amazon**, **Google**, **Microsoft**.

If you are creating a new reference:

* in _TTS Configuration — Provider_, type the name of the provider ("Amazon" or "Google" or "Microsoft")
* in _TTS Configuration — Locale_, choose the corresponding locale specified by the TTS provider
* in _TTS Configuration — Name_, type the official name of the voice specified by the TTS provider

## [Speech Recognizers](https://app.flowstorm.ai/#!/space/speechRecognizers)

Reference existing ASR (automatic speech recognition) settings from external providers. These speech recognizers can then be selected in [Personas](./#personas) or [Dialogues](./#dialogues).

Currently supported providers: **Amazon**, **Google**, **Microsoft**, **Deepgram**.

If you are creating a new reference, make sure all settings are compatible with your selected provider.
