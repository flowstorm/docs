---
description: >-
  This page describes how to download, configure and start Core and NLP services
  to run and develop Flowstorm Core.
---

# Project Setup

## Core services

{% hint style="warning" %}
You must have Java JDK version 8 and Apache Maven version 3 installed before you start with Core services. You cannot use newer versions of Java due to compatibility issues.&#x20;
{% endhint %}

```bash
git clone git@github.com:flowstorm/core.git
cd core
mvn install
```

### Google Application Credentials

Before you start Core services `GOOGLE_APPLICATION_CREDENTIALS` environment variable has to be set pointing to the Google Service Account key JSON file (otherwise Google Speech-To-Text and Text-To-Speech services used by Core runner won't work).&#x20;

### Application Configuration

You need also create `app.local.properties` configuration file and put it into working directory. It should contain at least `database.url` and `illusionist.key` properties.

| Property                         | Description                                                                                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Basic**                        |                                                                                                                                                                |
| `database.url`                   | <p><strong>Required</strong> MongoDB URL in form</p><p><code>mongodb+srv://user:pwd@db-server.net</code></p>                                                   |
| `namespace`                      | <p>Namespace to be used to construct Platform service URLs</p><p>Default value is <code>default</code></p>                                                     |
| `dsuffix`                        | Suffix for database collections                                                                                                                                |
| **NLP Services**                 | Platform NLP Services                                                                                                                                          |
| `illusionist.url`                | <p>URL of Illusionist (Flowstorm IR+NER Service). If not set following will be used</p><p><code>https://illusionist(.&#x3C;namespace>).flowstorm.ai</code></p> |
| `illusionist.key`                | Illusionist API key                                                                                                                                            |
| **Amazon Web Services**          | Required when using Amazon Polly Text-To-Speech service                                                                                                        |
| `aws.secret-key`                 | AWS secret key                                                                                                                                                 |
| `aws.access-key`                 | AWS access key                                                                                                                                                 |
| **Microsoft Cognitive Services** | Required when using Microsoft Text-To-Speech service                                                                                                           |
| `mscs.key`                       | Microsoft Cognitive Services API key                                                                                                                           |
| `mscs.location`                  | Microsoft Cognitive Services location (e.g. `westeurope`)                                                                                                      |
| **MailGun**                      | Optional service for sending e-mails from [DialogueScript](../../development/dialoguescript/)                                                                  |
| `mailgun.domain`                 | MailGun domain (e.g. `mg.your-server.com`)                                                                                                                     |
| `mailgun.apikey`                 | MailGun API key                                                                                                                                                |
| `mailgun.baseUrl`                | MailGun API base URL (e.g. [`https://api.eu.mailgun.net/v3/`](https://api.eu.mailgun.net/v3/))                                                                 |
| **ElasticSearch**                | Optional storage for storing and analysing conversational data                                                                                                 |
| `es.host`                        | ElasticSearch server host                                                                                                                                      |
| `es.user`                        | ElasticSearch user                                                                                                                                             |
| `es.password`                    | ElasticSearch password                                                                                                                                         |
| **Other (optional)**             |                                                                                                                                                                |
| `wcities.token`                  | wCities API access token                                                                                                                                       |
| `tmdb.key`                       | TMDb API key                                                                                                                                                   |

Finally you can start Core services by following commands to get runner available at localhost port 8080 and builder at port 8081

```bash
java -jar runner/app/target/app.jar
java -jar builder/app/target/app.jar -Dai.flowstorm.common.server.port=8081
```

## Docker images

If you want to build your own Docker images instead of using generic ones available at `registry.gitlab.com` you can do that by following commands:

```bash
docker build -t registry.your-server.com/flowstorm/runner/app runner/app
docker build -t registry.your-server.com/flowstorm/builder/app builder/app
```

## NLP Services

TBD

```bash
git clone git@gitlab.com:promethistai/illusionist.git
git clone git@gitlab.com:promethistai/duckling.git
```
