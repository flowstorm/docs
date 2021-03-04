---
description: How to make inbound and outbound calls using Twilio.com service.
---

# Twilio Calls

The platform supports both inbound and outbound phone calls via [Twilio.com](http://twilio.com/) platform.

{% hint style="info" %}
If you need to differentiate between inbound and outbound in main dialogue, use session.isInitiated property \(which indicated that session has been created from server initiative\) in functional code following `#intro` action
{% endhint %}

```kotlin
if (session.isInitiated) toOutbound else toInbound
```

## Inbound

To redirect calls to particular application or specific dialogue, you have to allocate phone number and link it to TwiML bin - see following example

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Response>
  <Connect>
    <Stream url="wss://core.flowstorm.com/call/">
      <Parameter name="locale" value="en-US" />
      <Parameter name="sender" value="{{From}}" />
      <Parameter name="appKey" value="<key>" />
    </Stream>
  </Connect>
</Response>
```

where `key` is valid conversational key \(:&lt;appId&gt; or :&lt;dialogueId&gt;\). Please note that in case of non-public application or particular dialogue unknown caller will be prompted to do pairing using spelled code first. To prevent this implicit behaviour, public application must be referenced in key \(in that case any caller will be represented by anonymous user object in platform\).

Starting platform version 2.9, you can also let platform define TwiML for particular application instead of using custom TwiML bin. Just use URL [https://admin.promethist.com/initiations/call](https://admin.promethist.com/initiations/call)/&lt;appId&gt; as incoming call web hook using GET method.![](blob:https://promethistai.atlassian.net/ca625d8c-7b9e-4a63-b429-892494b94f81#media-blob-url=true&id=4c3aeba5-02fb-43bf-a520-19e699388d86&collection=contentId-834109451&contextId=834109451&mimeType=image%2Fpng&name=image-20200914-094705.png&size=86037&width=1114&height=350)

![](../../.gitbook/assets/image%20%2816%29.png)

## Outbound

Call from particular application of specific dialogue can be initiated by standalone \(CLI\) client or from web interface \(TODO\). Twilio Account SID, Auth Token and phone number with outbound calls allowed for specified geolocations \(see [https://www.twilio.com/console/voice/calls/geo-permissions/](https://www.twilio.com/console/voice/calls/geo-permissions/)\) are required.

### Standalone

Standalone client can be used to initiate outbound call

```bash
promethist call --account <accountSID> --token <authToken> --to <recipientPhoneNumber> --from <callerPhoneNumber> --key <key>
```

{% hint style="info" %}
Phone numbers have to start with + sign \(e.g. +420608123456\)
{% endhint %}

### Web

[app.flowstorm.ai](https://app.flowstorm.ai)[ ](https://app.flowstorm.ai/#!/space)&gt; New &gt; Session Initiation OR Data &gt; Sessions &gt; + Initiate![](blob:https://promethistai.atlassian.net/f8d6bbd9-053a-4cc2-b260-ab62a9ee4c9a#media-blob-url=true&id=ad263248-c80b-4fe2-bfe3-f16da61118c5&collection=contentId-834109451&contextId=834109451&mimeType=image%2Fpng&name=image-20200912-135646.png&size=81019&width=794&height=431)

![](../../.gitbook/assets/image%20%2817%29.png)

{% hint style="warning" %}
Before initiating session please in ensure that a\) application linked to initiated session has set valid Twilio phone number which as allowed outbound calls for user geolocation \(Access &gt; Applications\) and b\) your organization has set valid combination of Twilio account SID and auth token \(Main &gt; Organization\).
{% endhint %}

