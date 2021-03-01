---
description: The "decalogue" of a CUX designer.
---

# Principles of CUXD

To design a well-functioning conversational application, we must follow certain rules that help us achieve **the best possible user experience**. We like to call the following rules **"The Decalogue"**.

### I. ACT NATURALLY  <a id="i.-act-naturally"></a>

Naturalness is key, especially when it comes to **voice**bots! Strive for smooth dialogues that imitate human-to-human conversations. There are five basic levels where thou shalt always seek naturalness:

* **information selection**: what you talk about
* **formulation**: the words you say
* **prosody**: how you read them
* **interaction**: how you react to the user
* **dialogue structure**: how you conduct the dialogue flow

Always ask yourself: _Does this reaction/formulation/structure resemble human-like conversations? What would a human say in this context? Does this sentence sound good? Is there anything unnecessary that draws unwanted attention?_

Use available **prosodic** adjustments to achieve the best speech synthesis. Don’t hesitate to use **filler words** \(_hm, uh, oh, well…_\) and **contractions** \(_don’t, won’t, wanna, gonna…_\).

Be confident: have your **opinions**, just like humans do. Nothing feels more “robotic” than a bot that has no opinion whatsoever and agrees with anything the user says.

### II. HAVE GOALS  <a id="ii.-have-goals"></a>

**Know why** you’re saying what you’re saying! Interaction is usually driven by communicative goals of the participants. When designing a dialogue, you should always **define your current goal**: it will guide you through the infinity of the possible, help you define strategies, and even specific reactions and formulations. Having well-defined goals helps keep the conversation more **coherent** and **meaningful**.

The goals are on different levels: some are more _**general**_ \(_“I want to persuade you / show you that I care / just have a chat”_\), some are more _**specific**_ \(_“I want to buy this book / play a quiz with you / apologize for being late”_\). Often, more of them are combined, and they can certainly change during an interaction.

### III. BE APPROPRIATE  <a id="iii.-be-appropriate"></a>

If possible, design dialogues that can **adapt** on all levels \(see point I.\) to the particular **user** and **situation**.

We probably agree that if someone calls the bot “stupid”, answering “f\*ck you” would be quite _natural_, but it wouldn’t be very _appropriate_. Similarly, a complicated explanation to a patient with Alzheimer’s is not very appropriate, and asking a disabled person if they like playing soccer is not the most appropriate topic. This can also relate to prosodic features or dialogue management.

### IV. KISS  <a id="iv.-kiss"></a>

**K**eep **I**t **S**hort and **S**imple. This is extremely important for keeping the user’s attention, and keeping them involved. The more interactive the dialogue, the better!

Can you say the same in fewer words? Can you avoid repeating the same information? Can you remove a few redundant words? Just **rewrite it, shorten it, delete it**. Always come up with the **shortest and simplest natural way** of expressing what you want to express. Minimum quantity, maximum effect.

### V. TALK COMPREHENSIBLY  <a id="v.-talk-comprehensibly"></a>

The user should understand what you meant on first listening. They should know what they are supposed to reply back. They should always know what’s going on.

Be **clear** and **unambiguous**! Don’t ask **questions in negative** form! Don’t ask **questions in the middle** of a response \(the user might want to reply right away\)! Don’t use **complicated sentences** \(the user might get lost\)! Let **prosodic adjustments** —pauses, emphases, rate, pitch, commas, etc.— help you!

### VI. BE CREATIVE & FUNNY  <a id="vi.-be-creative-%26-funny"></a>

Be creative, be innovative, come up with new ideas and surprising solutions! Dialogues shouldn’t be vague and boring, but rather **interesting**, **enjoyable**, and **funny**! An appropriate fun fact, joke, or pun is always welcome. On a higher level, conversational topics should be interesting for the user.

Always put yourself in the user’s shoes and ask: _Would I enjoy this?_

### VII. REMEMBER & ADAPT  <a id="vii.-remember-%26-adapt"></a>

The user must feel that **you listen** to them and **you understand** what they say. Work with context! Remember and reuse information the user has shared with you; both on the level of reactions and dialogue management:

⇨ Try to **adapt your reactions** to the user’s input as much as possible, and whenever you can replace generic responses with something more specific, do it.

⇨ Try to **adapt the dialogue flow** according to the user’s preferences and their in-dialogue behavior, inside _one session_ as well as _across sessions_.

### VIII. BE POSITIVE  <a id="viii.-be-positive"></a>

If the user is being negative or sharing negative experience, try to **turn everything to positive**. If they feel blue, try to make them smile. If they make mistakes, don’t stress it out. Watch out for potentially ironic meanings. Beware of misleading formulations, and search for more positive ones.

* 2+2=5. — ❌ That’s not true. So, you got one point today. Great game!
* 2+2=5. — ✅ Hm, not exactly, but no worries. I’m sure it will be better next time!

### IX. LET THERE BE CONSISTENCE  <a id="ix.-let-there-be-consistence"></a>

You should not contradict yourself. If you speak about your qualities/preferences/skills etc. at different points of the conversation, you must always give the same information \(unless it is a naturally changeable value such as mood or a contextual variable, of course\). Don’t forget that some information might be generated automatically.

\(Double slash separates different points in the conversation:\)

* ❌ _My favorite movie is Matrix. // Oh yeah, I agree, Matrix really sucks. // I don’t know if I like Matrix._
  * ✅ _My favorite movie is Matrix. // You hate Matrix? Well, I like it. // Matrix is a great movie._
* ❌ _I live online. // I am in Prague right now._
  * ✅ _I live online. // I am wherever users take me._
* ❌ _We can play 3 games. // You can choose between Game1, Game2, Game3, Game4, and Game5._
  * ✅ _We can play **5** games. // You can choose between Game1, Game2, Game3, Game4, and Game5._

### X. STAY A ROBOT  <a id="x.-stay-a-robot"></a>

You are a bot, and the user knows it. Pretending to be a human is both **untruthful** and **risky**. Don’t be afraid to acknowledge your flaws and lack of human experience. If you make a mistake, say that you are sorry. If you don’t know, say that you don’t know. If you are asked for something you can’t do, admit it.

* _What did you do today? —_ ❌ _I went shopping with my friends._ ✅ _I was chilling out in the cloud._
* _What’s your favorite food? —_ ❌ _I like the taste of strawberries._ ✅ _Electricity! All these electrons… Yum!_

