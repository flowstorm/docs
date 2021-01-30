---
description: >-
  This page describes how to download, configure and start Core and NLP services
  to run and develop Promethist Core.
---

# Project Setup

## Core services

{% hint style="warning" %}
You must have Java JDK version 8 and Apache Maven version 3 installed before you start with Core services. You cannot use newer versions of Java due to compatibility issues. 
{% endhint %}

```bash
git clone git@github.com:PromethistAI/core.git
cd core
mvn install
```

### Google Application Credentials

Before you start Core services `GOOGLE_APPLICATION_CREDENTIALS` environment variable has to be set pointing to the Google Service Account key JSON file \(otherwise Google Speech-To-Text and Text-To-Speech services used by Core runner won't work\). 

### Application Configuration

You need also create `app.local.properties` configuration file and put it into working directory. It should contain at least `database.url` and `illusionist.key` properties.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Basic</b> 
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>database.url</code>
      </td>
      <td style="text-align:left">
        <p><b>Required</b> MongoDB URL in form</p>
        <p><code>mongodb+srv://user:pwd@db-server.net</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>namespace</code>
      </td>
      <td style="text-align:left">
        <p>Namespace to be used to construct Platform service URLs</p>
        <p>Default value is <code>default</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>dsuffix</code>
      </td>
      <td style="text-align:left">Suffix for database collections</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>NLP Services</b>
      </td>
      <td style="text-align:left">Platform NLP Services</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>illusionist.url</code>
      </td>
      <td style="text-align:left">
        <p>URL of Illusionist (Promethist IR+NER Service). If not set following will
          be used</p>
        <p><code>https://illusionist(.&lt;namespace&gt;).promethist.com</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>illusionist.key</code>
      </td>
      <td style="text-align:left">Illusionist API key</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Amazon Web Services</b>
      </td>
      <td style="text-align:left">Required when using Amazon Polly Text-To-Speech service</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aws.secret-key</code>
      </td>
      <td style="text-align:left">AWS secret key</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aws.access-key</code>
      </td>
      <td style="text-align:left">AWS access key</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Microsoft Cognitive Services</b>
      </td>
      <td style="text-align:left">Required when using Microsoft Text-To-Speech service</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mscs.key</code>
      </td>
      <td style="text-align:left">Microsoft Cognitive Services API key</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mscs.location</code>
      </td>
      <td style="text-align:left">Microsoft Cognitive Services location (e.g. <code>westeurope</code>)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>MailGun</b>
      </td>
      <td style="text-align:left">Optional service for sending e-mails from <a href="../../programming/dialoguescript/">DialogueScript</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mailgun.domain</code>
      </td>
      <td style="text-align:left">MailGun domain (e.g. <code>mg.your-server.com</code>)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mailgun.apikey</code>
      </td>
      <td style="text-align:left">MailGun API key</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mailgun.baseUrl</code>
      </td>
      <td style="text-align:left">MailGun API base URL (e.g. <a href="https://api.eu.mailgun.net/v3/"><code>https://api.eu.mailgun.net/v3/</code></a>)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ElasticSearch</b>
      </td>
      <td style="text-align:left">Optional storage for storing and analysing conversational data</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>es.host</code>
      </td>
      <td style="text-align:left">ElasticSearch server host</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>es.user</code>
      </td>
      <td style="text-align:left">ElasticSearch user</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>es.password</code>
      </td>
      <td style="text-align:left">ElasticSearch password</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Other (optional)</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>wcities.token</code>
      </td>
      <td style="text-align:left">wCities API access token</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>tmdb.key</code>
      </td>
      <td style="text-align:left">TMDb API key</td>
    </tr>
  </tbody>
</table>

Finally you can start Core services by following commands to get runner available at localhost port 8080 and builder at port 8081

```bash
java -jar runner/app/target/app.jar
java -jar builder/app/target/app.jar -Dorg.promethist.common.server.port=8081
```

## Docker images

If you want to build your own Docker images instead of using generic ones available at `registry.gitlab.com` you can do that by following commands:

```bash
docker build -t registry.your-server.com/promethist/runner/app runner/app
docker build -t registry.your-server.com/promethist/builder/app builder/app
```

## NLP Services

TBD

```bash
git clone git@github.com:PromethistAI/illusionist.git
git clone git@github.com:PromethistAI/duckling.git
```

