---
description: >-
  The component called "Skimmer" allows you to look for information that the
  user mentioned "by the way", and store it into a user or session profile.
---

# Skimmer

## Why you need Skimmer

Imagine that your digital persona has the following conversation with a user:

`Persona: Do you like movies?  
User: Yes, I love movies. My sister and I organize movie nights every Friday.`

You might notice multiple pieces of information that we can learn about the user from what they said. The first is that they _love movies_. This information can be saved into the user profile simply using dialogue design. You can branch the dialogue with two local intents and connect the functional nodes that save the corresponding values into the user profile \(e.g. `userProfile.movies.isMovieFan = true`\):

![Saving information about the user into the user profile using conversation design.](https://lh4.googleusercontent.com/uWKY-9vuQL7VrEo3GBa_s3qgmbOwtZXjciIrmdvPS7B6QgjVj-FfVU5ueUL5EZL7bMjTD_d8rrv0pzGktN1DUVa0hjITEEpEk9TI4Wz19_jXDtrXeELQMlJdFmCxV5n6EM7uNm_I=s0)

However, the user also mentioned their _sister_—so we can infer that _the user has a sister_, and we might want to save this information for future use: but standard conversation design is not prepared to save such information easily. The general problem is that the user can mention all kinds of additional pieces of information \(concerning e.g. their family members, pet, or favorite activities\), and even worse, they can do that literally at any point in the conversation. That's why handling such situations manually is not feasible.

{% hint style="info" %}
Users can mention various pieces of information in the dialogue where the digital persona doesn't ask for them explicitly. **Skimmer** allows the persona to recognize and remember such information for later use.
{% endhint %}

## How Skimmer works

Skimmer is an NLU component that analyses each user input and finds pieces of information that the dialogue designer specifies.

{% hint style="info" %}
Skimmer is defined **per application**. This means that the same Skimmer works in all subdialogues of the application.
{% endhint %}

Skimmer looks for information defined in a JSON file. It has the folowing structure:

```text
[
      {
        "type": "regex",
        "pattern": "((?<!death of )(my (sister|sis|stepsister|half-sister))(?!( died| passed away| is dead)))",
        "path": "userProfile.family.sisterMentioned",
        "value": true
      },
      {
        "type": "regex",
        "pattern": "(I (have been feeling|am feeling|feel|am doing) (bad|terrible|angry|exhausted|tired|awful|lonely|sad|stressed|depressed|overwhelmed|bad mood|pissed off|not too well|not well|bored))",
        "path": "session.emotions.feelBad",
        "value": true
      },
      {
        "type": "regex",
        "pattern": "((?<!death of )(my (?<pet>dog|cat|rabbit))(?!( died| passed away| is dead)))",
        "path": "userProfile.pets.petMentioned",
        "value": "?<pet>"
      }
]

```

The JSON file contains a list of rules. Each rule has a type, pattern, path, and value. The type specifies the type of the rule. Skimmer currently supports _regex only_. The pattern contains the regex rule that Skimmer tries to match in each user input. The path specifies the attribute that Skimmer will modify. And the value specifies which value will be saved into the attribute once the pattern is matched.

## **Examples of Rules**

We can divide the rules into two categories—those that save the predetermined value and those that save part of the regex.

### **Rule to Save Predetermined Value**

You might want to save a predefined value into the user profile. The example can be a boolean value `true` if the user mentions their sister.

```text
{
   "type": "regex",
   "pattern": "((?<!death of )(my (sister|sis|stepsister|half-sister))(?!( died| passed away| is dead)))",
   "path": "userProfile.family.sisterMentioned",
   "value": true
}
```

The following parts of the regex are called _negative lookahead_ and _negative lookbehind_:

`(?<!death of )  
(?!( died| passed away| is dead))`

They are helpful in cases when you don’t want the regex to be triggered in some contexts. The rule above will be found if the user inputs _"I like **my mother**"_, for example. But it won't be found if the user inputs _"I'm sad due to the recent death of my mother"_ or _"My mother died today"_ thanks to the negative lookahead and negative lookbehind. This is a vital aspect of the emotional intelligence your digital persona should possess.

We can also save string values as in the following example.

```text
{
   "type": "regex",
   "pattern": "I grow up in city",
   "path": "userProfile.person.grewUpPlace",
   "value": "city"
}
```

### **Rule to Save Part of Regex**

What if we want to save part of the regex? For example when the user mentions their pet animal. We can use the regex named groups and use the name of the group as the value.

`(?<pet>dog|cat|rabbit)`

Skimmer has recently started to support one named group in the rule only.

```text
{
   "type": "regex",
   "pattern": "((?<!death of )(my (?<pet>dog|cat|rabbit))(?!( died| passed away| is dead)))",
   "path": "userProfile.pets.petMentioned",
   "value": "?<pet>"
}
```

If the user inputs _"I like my **dog**"_ the value "dog" will be saved.

## **Using Skimmer**

To start using Skimmer in your application, follow these steps:

* Create a JSON file containing a list of rules.
* Upload the JSON file into **Design &gt;&gt; Files** as a new file. Copy the URL of the file.

![Upload the JSON file in Design &amp;gt;&amp;gt; Files.](https://lh3.googleusercontent.com/tx4_Mn-t1AE5atBrwoGR3Mq1drsiW2ay8CG29xplNWrurd3ip38VFSP1aQQMEyZTR_1PH7zeTXqUgoYA9XoslHSFA6L_S8V_xNefA1iPDxpeSrdWQL7l9rfsFeNlxLpnUvx0e_Uw=s0)

* Set the URL of the JSON file with the rules as a Skimmer URL in **Access &gt;&gt; Applications &gt;&gt; Skimmer**.

![Set the URL of the JSON file containing rules in Access &amp;gt;&amp;gt; Applications &amp;gt;&amp;gt; Skimmer.](https://lh5.googleusercontent.com/qGRi-5eel2RRhHimq1IcVyKh1yOoUAbbIOnqFtxU85jdjryEkGdlnJ2gW3z4C64d_QAgX5GWnLmlKkz4gWZ2tKd8ctYSD0tO6wTY0B9Mummn8xAKHGIj-FztLlv-u2MjHZEd6WGO=s0)



