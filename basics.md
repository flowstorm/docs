---
description: >-
  What you should know about today's conversational AI. If you're new to
  conversational AI, be sure to read this article before you throw yourself into
  creating!
---

# Basics

When you talk to a machine, it's usually not (yet) the same experience as if you chatted with a good friend of yours. Even though AI researchers and developers all over the world strive for making conversational AI as human-like as possible, technology still has certain limitations that are reflected in the interaction. On the other hand, as the use of "talking machines" increases, people are getting used to the differences and constantly modify their expectations for future interactions.

So what can you and your end users generally expect from a state-of-the-art conversational application, just like one created in **Flowstorm**? What are the basic principles and general shortcomings?

### **Different components take care of different tasks.**

What happens when you're talking to someone? When they say something, your _ears_ catch the sound waves. Afterward, your _brain_ analyzes the sound wave and interprets the meaning of the resulting words; then it comes up with the most suitable reaction. Finally, your _speech organs_ articulate the message. Conversational systems have these different organs/components, too, although they are a bit more independent:

* _**ASR**_ (Automatic Speech Recognition) is like the system's ears (and partially the brain). It's a component that detects the sound wave and transcribes it into words.
* _**NLP**_ (Natural Language Processing) algorithms are the brain. They analyze the meaning of the words and, based on the context, generate the most suitable reaction in the form of written text.
* _**TTS**_ (Text-To-Speech) is the system's speech organ. It takes the written text and "reads it out loud", in a specific voice.
* And, following this analogy, the whole architecture interconnecting all the other components would be the _body_.

![Um... you know what we mean.](<.gitbook/assets/image (75).png>)

It's important to keep this split in mind, especially when analyzing the mistakes of a bot. Problems may originate from any of these components. It's not always the fault of the brain - perhaps your partner just misheard what you said!

### **Conversations are based on the so-called "turn-taking principle".**

This means that the bot and the user _take turns_ in saying stuff. Each line enters the conversation as a whole, at once: much like in written chat, you receive the whole message only when it is finished. So while you're talking, there's usually no real-time analysis of the input until you finish. You can imagine the succession like this:

* _The bot says something, then starts listening._
* _The user says something._
* _The bot stops listening, analyzes the response, then says something, and then starts listening._
* etc.

Most current voice systems work with the so-called _half-duplex_ (_semiduplex_) mode: the bot is either speaking or listening, usually signaling somehow the transition between the states. Another listening mode is the _full-duplex_: the bot is listening to you even while it is speaking, so you can barge in just by starting talking. Full-duplex can be used also on certain devices that run Flowstorm apps.

### The interaction can be multimodal.&#x20;

A conversational application doesn't have to be just about words. Very often, you can interact via voice, text, or touch; and you can expect to not only hear the voice but also listen to sounds, see images, watch videos, or even talk to a 3D avatar!
