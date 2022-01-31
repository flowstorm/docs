---
description: >-
  Flowstorm Sockets are used to connect stateful client application to
  Flowstorm.
---

# Flowstorm Sockets

## Client states

Every client should distinguish among following states. &#x20;

| Name         | Description                                                                                                                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Closed`     | Socket connection to server is not open (this is the initial state of the client)                                                                                                                            |
| `Open`       | Socket connection is ready                                                                                                                                                                                   |
| `Failed`     | connection failed - in that case client should repeatedly try to connect; in common implementation, every 10 seconds by default                                                                              |
| `Sleeping`   | no conversation session is going on (session has not started yet or session has been finished)                                                                                                               |
| `Listening`  | bot is waiting for user input (input audio and/or text, depending on client configuration)                                                                                                                   |
| `Processing` | user input has been received by server and is being processed by server                                                                                                                                      |
| `Responding` | response is sent to the client - after all the output audio (speech synthesis)/video is played (or skipped by user), client goes back to `Listening` state (is session has not ended) or to `Sleeping` state |
