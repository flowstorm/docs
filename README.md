---
description: Unleash The Power of Spoken Word
---

# What is PromethistAI

The Promethist AI Platform is a set of SaaS development tools and software components for creating smart Conversational & Multimodal AI applications. The Platform has two major components: the [Web Application](app/welcome.md) and the [Core](core/about-project.md) open source project. Creators will use the web application provided at URL [promethist.app](./) to author the dialogues, and administrators will use it to manage the applications. The Core contains all runtime components. 

The critical elements are the built-in Voice First development workflow and reusability. We guide the developer through all stages, starting from the scenario to the analytics. For fast application authoring, the Platform introduces the following reusable components: [sub-dialogues](app/basic-use-cases/sub-dialogues.md), [mixins]() and [snippets](app/building-blocks/snippets.md).

{% embed url="https://www.youtube.com/watch?v=Tv8CQrJoeKI" %}

The principal part of a conversational application are dialogues, and the [Dialogue Designer](app/working-space/design/dialogue-designer.md) is an essential tool to create and visualise them as graphs of nodes. The nodes contain [speech](app/basic-use-cases/speech-output.md) prompts, nodes with expected replies, and code including nodes. Domain-Specific Language \(DSL\) [DialogueScript](programming/dialoguescript/) built on top of [Kotlin programming language](https://kotlinlang.org/) controls the dialog flow, access to data and APIs, natural language generation, etc.

The [Core](core/about-project.md) executes [dialogue models](app/building-blocks/dialogue-models.md). It may run in the cloud or on-premise. The internet-connected [platform clients](clients/introduction.md) use a selection of ASR and TTS \(Microsoft, Google, and Amazon\). The Core also collects the sessions to support analytics.

We offer a variety of  Clients for mobile, web, Alexa, and Google Hub. Also, we provide a [standalone client](clients/standalone/) to voice-enable any HW with a microphone and speaker. Platform customers have designed clients for in-car, Virtual Reality headset, and other devices. 

![](.gitbook/assets/image%20%287%29.png)

The Platform also support access to sensors and actuators. The context stores the sensors’ states. Sensors may invoke a conversation or issue commands to actuators.

To create advanced, adaptive, and personalised Voice First applications, the rich context is essential. We provide different scopes of automatically maintained context: The [Session](app/context-scopes/session.md) Context variables persist during a session. The [User](app/context-scopes/user.md) Context stores users’ profile values. The [Community](app/context-scopes/community.md) Context allows sharing between communities of users. 

Platform web application comes with a robust set of administration tools. Administrators manage spaces with users and devices. Users can be organised into groups with defined sets of supported applications. The web application also provides analytics. 

An essential part of the Core is the NLP pipeline. It is a state-of-the-art set of algorithms implemented as set of web services. The NLP pipeline offers different algorithms, such as intent, named entity recognition, etc. Developers may easily add custom NLP services.

[Get started for free](https://promethist.app/#!/signup) and [make your first steps](quick-start.md) with Promethist.

