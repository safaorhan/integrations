[npm]: https://img.shields.io/badge/npm-broid-green.svg?style=flat
[npm-url]: https://www.npmjs.com/org/broid

[node]: https://img.shields.io/node/v/@broid/slack.svg
[node-url]: https://nodejs.org

[deps]: https://img.shields.io/badge/dependencies-checked-green.svg?style=flat
[deps-url]: #integrations

[tests]: https://img.shields.io/travis/broidHQ/integrations/master.svg
[tests-url]: https://travis-ci.org/broidHQ/integrations

[bithound]: https://img.shields.io/bithound/code/github/broidHQ/integrations.svg
[bithound-url]: https://www.bithound.io/github/broidHQ/integrations

[bithoundscore]: https://www.bithound.io/github/broidHQ/integrations/badges/score.svg
[bithoundscore-url]: https://www.bithound.io/github/broidHQ/integrations

[nsp-checked]: https://img.shields.io/badge/nsp-checked-green.svg?style=flat
[nsp-checked-url]: https://nodesecurity.io

[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![bithound][bithound]][bithound-url]
[![bithoundscore][bithoundscore]][bithoundscore-url]
[![nsp-checked][nsp-checked]][nsp-checked-url]

# Broid GroupMe Integration

Broid Integrations is an open source project providing a suite of Activity Streams 2 libraries for unified communications among a vast number of communication platforms.

> Connect your App to Multiple Messaging Channels with  One OpenSource Language.

[![gitter](https://badges.gitter.im/broidHQ/broid.svg)](https://t.broid.ai/c/Blwjlw?utm_source=github&utm_medium=readme&utm_campaign=top&link=gitter)
[![slackin](https://slackin.broid.ai/badge.svg)](https://slackin.broid.ai)

## Message types supported

| Simple | Image | Video | Buttons | Location | Phone number |
|:------:|:-----:|:-----:|:-------:|:--------:|:------------:|
|   ✅    |   ✅   |   ✅   |         |    ✅     |              |

_Buttons, Phone number are platform limitations._

_Videos are supported as a simple message_

## Getting started

### Install

```bash
npm install --save @broid/groupme
```

### Connect to Groupme

```javascript
const BroidGroupme = require('@broid/groupme');

const groupme = new BroidGroupme({
  username: '<your_sender_number>',
  token: '<your_groupme_token>',
  tokenSecret: '<your_groupme_token_secret>',
  http: {
    port: 8080,
    host: "0.0.0.0"
  }
});

groupme.connect()
  .subscribe({
    next: data => console.log(data),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });
```

Groupme can also be used with your existing express setup.

```javascript
const BroidGroupme = require('broid-groupme');
const express = require("express");

const groupme = new BroidGroupme({
  username: '<your_sender_number>',
  token: '<your_groupme_token>',
  tokenSecret: '<your_groupme_token_secret>',
});

const app = express();
app.use("/groupme", groupme.getRouter());

groupme.connect()
  .subscribe({
    next: data => console.log(data),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });

app.listen(8080);
```

**Options available**

| name            | Type     | default    | Description  |
| --------------- |:--------:| :--------: | --------------------------|
| serviceID       | string   | random     | Arbitrary identifier of the running instance |
| logLevel        | string   | `info`     | Can be : `fatal`, `error`, `warn`, `info`, `debug`, `trace` |
| username        | string   |            | Your bot name |
| token           | string   |            | Your groupme bot id         |
| tokenSecret     | string   |            | Your groupme access token  |
| http            | object   | `{ "port": 8080, "http": "0.0.0.0" }` | WebServer options (`host`, `port`) |

### Receive a message

```javascript
groupme.listen()
  .subscribe({
    next: data => console.log(`Received message: ${data}`),
    error: err => console.error(`Something went wrong: ${err.message}`),
    complete: () => console.log('complete'),
  });
```

### Post a message

To send a message, the format should use the [broid-schemas](https://github.com/broidHQ/integrations/tree/master/broid-schemas).

```javascript
const formatted_message = {
  "@context": "https://www.w3.org/ns/activitystreams",
  "type": "Create",
  "generator": {
    "id": "f6e92eb6-f69e-4eae-8158-06613461cf3a",
    "type": "Service",
    "name": "groupme"
  },
  "object": {
    "type": "Note",
    "content": "hello world"
  },
  "to": {
    "id": "28728284",
    "type": "Person"
  }
};

groupme.send(formatted_message)
  .then(() => console.log("ok"))
  .catch(err => console.error(err));
```

## Examples of messages

You can find examples of sent and received messages at [Broid-Schemas](https://github.com/broidHQ/integrations/tree/master/broid-schemas).

## Contributing to Broid

See [CONTRIBUTE.md](../CONTRIBUTE.md)

## Copyright & License

Copyright (c) 2016-2017 Broid.ai

This project is licensed under the AGPL 3, which can be
[found here](https://www.gnu.org/licenses/agpl-3.0.en.html).
