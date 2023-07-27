# Core Application

Core application provides web runtime following services

* [Communication channels ](client-integrations/)
  * [Amazon Alexa REST](client-integrations/amazon-alexa-rest.md) endpoint at `/alexa`
  * [Google Assistant REST](client-integrations/google-app-rest.md) endpoint at `/google`
  * [Twilio Stream Socket](client-integrations/twilio.md) at `/call`&#x20;
  * [Flowstorm Socket V1](client-integrations/flowstorm-sockets/web-socket.md) at `/socket`&#x20;
  * [Flowstorm Socket V2](client-integrations/flowstorm-sockets/flowstorm-socket-v2.md) at `/client`
* NLP pipeline and services
  * Dialogue Manager
  * STT (ASR) and other input streams
  * TTS services
  * Embedded Core NLU components
    * Dialogue Selector
    * Tokenizer and Normalizer
    * [Skimmer](nlp-services/skimmer.md)
    * Profanity Filter
    * Simple IR and NER (for testing purposes only)
  * Illusionist IR and NER
  * Duckling for specific NER needs
  * Triton for sentiment analysis
  * NRG (Neural Response Generator)
