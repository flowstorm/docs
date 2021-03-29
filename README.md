---
description: Unleash the power of spoken word!
---

# What is Flowstorm

## **Flowstorm is...**

... a platform where _anyone_ can create, design, manage and analyze complex voice-first multimodal **conversational AI applications**. [Create your first app in a few minutes](quick-start.md) and you'll be thrilled to [design more and more advanced AI personas]()!

{% embed url="https://www.youtube.com/watch?v=Tv8CQrJoeKI" %}

## Tell me more!

The Flowstorm platform is a set of SaaS development tools and software components. It has two major components: the [Web Application](app/welcome.md) and the [Core](core/about-project.md) open source project. Conversation designers and developers will use the web application provided at [app.flowstorm.ai](https://app.flowstorm.ai/) to design the dialogues; administrators will use it to manage the applications. The Core contains all runtime components.

The critical elements are the built-in Voice First development workflow and reusability. We guide the developer through all stages, from the dialogue draft to analytics. For fast application authoring, Flowstorm introduces the following reusable components: subdialogues, mixins, and snippets.

The principal components of a conversational application are dialogues, and the [Dialogue Designer](app/space/design/dialogue-designer.md) is an essential tool to create and visualize them as graphs of nodes. The nodes contain [speech](model/dialogue-model-coding/basic-use-cases/speech-output.md) prompts, nodes with expected replies, and nodes with code. Our Domain-Specific Language \(DSL\) called [DialogueScript](model/dialoguescript/), built on top of [Kotlin programming language](https://kotlinlang.org/), controls the conversational flow, the access to data and APIs, natural language generation, etc.

The [Core](core/about-project.md) executes [dialogue models](model/dialogue-model-coding/building-blocks/dialogue-model.md). It can run in the cloud or on-premise. The internet-connected [platform clients](clients/introduction.md) use a selection of ASR and TTS \(Microsoft, Google, and Amazon\). The Core also collects the sessions to support analytics.

We offer a variety of clients for mobile, web, Alexa, and Google Hub. We also provide a [standalone client](clients/standalone/) to voice-enable any hardware with a microphone and a speaker. Flowstormers have successfully designed clients for the in-car environment, virtual reality headsets, and other devices. 

![](.gitbook/assets/image%20%287%29.png)

Flowstorm also supports access to sensors and actuators. The sensors’ states are stored in the context. Sensors may invoke a conversation or issue commands to actuators.

To create advanced, adaptive, and personalized voice-first applications, rich **context** is essential. We provide different scopes of automatically maintained context: the [_Session_](model/dialogue-model-coding/context-scopes/session.md) _context_ variables persist in a session. The [_User_](model/dialogue-model-coding/context-scopes/user.md) _context_ stores users’ profile values. The [_Community_](model/dialogue-model-coding/context-scopes/community.md) _context_ allows sharing values across a whole community of users.

The Flowstorm web application comes with a robust set of **administration tools**. Administrators manage Spaces with collaborators, end users, or devices. Users can be organized into groups with defined sets of supported applications. The web application also provides analytics. 

An essential part of the Core is the **NLP pipeline**. It is a state-of-the-art set of algorithms implemented as a set of web services. The NLP pipeline offers different algorithms, such as intent recognition, named entity recognition, etc. Developers can easily add custom NLP services.

[Get started for free](https://app.flowstorm.ai/) and [make your first steps](quick-start.md) with Flowstorm!

