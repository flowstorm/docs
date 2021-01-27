# Introduction

The Promethist AI \(PAI\) Platform is a set of SaaS development tools and software components for creating smart Conversational & Multimodal AI applications. The PAI Platform has two major components: the UI component and the Core. Creators will use the graphical UI component to author the dialogs, and administrators will use it to manage the applications. The Core contains all runtime components. 

The critical elements of the PAI are the built-in Voice First development workflow and reusability. We guide the developer through all stages, starting from the scenario to the analytics. For fast application authoring, the PAI introduces the following reusable components: sub-dialogs, mixins, and code snippets.

The principal part of a conversational application are dialogs, and the PAI Dialogue Designer is an essential tool to create and visualize them as graphs of nodes. The nodes contain TTS  prompts, nodes with expected replies, and code including nodes. Domain-Specific Language \(DSL\) or a Kotlin code controls the dialog flow, access to databases, natural language generation, etc.

The PAI Core interprets the dialogs graphs. It may run in the cloud or on-premise. The PAI internet-connected Clients use a selection of ASR and TTS \(MS, Google, and Amazon\). The Core also collects the sessions to support analytics.

We offer a variety of PAI Clients for mobile, WEB, Alexa, and Google Hub. Also, we provide a sample client code to voice-enable any HW with a microphone and speaker. Our partners have designed clients for in-car, Virtual Reality headset, and other devices. 

The PAI Core and Dialogue Designer also support access to sensors and actuators. The PAI context stores the sensors’ states. Sensors may invoke a conversation or issue commands to actuators.

To create advanced, adaptive, and personalized Voice First applications, the rich context is essential. We provide different scopes of automatically maintained context: The Session Context variables persist during a session. The User Context stores users’ profile values. The Community Context allows sharing between communities of users. 

Our platform comes with a robust set of administration tools. PAI Administration manages spaces with users and devices. Users are organized in groups with defined sets of supported applications. The Administration module also provides analytics. 

An essential part of the PAI Core is the NLP pipeline. It is a state-of-the-art set of algorithms implemented as microservices. The NLP pipeline offers different algorithms, such as intent, named entity recognition, etc. Developers may easily add custom NLP algorithms.



