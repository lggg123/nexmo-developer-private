---
title: Create a webhook server
description: In this step you learn how to create a suitable webhook server that supports an inbound call from a PSTN phone to a web app.
---

# Create a webhook server

When an inbound call is received, Vonage makes a request to a publicly accessible URL of your choice - we call this the `answer_url`. You need to create a webhook server that is capable of receiving this request and returning an [NCCO](/voice/voice-api/ncco-reference) containing a `connect` action that will forward the call to the [PSTN phone number](/concepts/guides/glossary#virtual-number). You do this by extracting the destination number from the `to` query parameter and returning it in your response.

## New project

Create a new project directory in a destination of your choice and change into it:

``` bash
mkdir app-to-phone-kotlin
cd app-to-phone-kotlin
```

Inside the folder, initialize a new Node.js project by running this command:

``` bash
npm init -y
```

## Add dependencies

Next, install the required dependencies:

``` bash
npm install express localtunnel --save
```

## Create the server file

Inside your project folder, create a file named `server.js` and add the code as shown below - please make sure to replace  `NUMBER` with your Vonage number (in [E.164](https://en.wikipedia.org/wiki/E.164) format), as well as `SUBDOMAIN` with an actual value. The value used will become part of the URLs you will set as webhooks in the next step.

``` javascript
'use strict';

const subdomain = 'SUBDOMAIN';
const vonageNumber = 'NUMBER';

const express = require('express')
const app = express();
app.use(express.json());

app.get('/voice/answer', (req, res) => {
  console.log('NCCO request:');
  console.log(`  - callee: ${req.query.to}`);
  console.log('---');
  res.json([ 
    { 
      "action": "talk", 
      "text": "Please wait while we connect you."
    },
    { 
      "action": "connect",
      "from": vonageNumber,
      "endpoint": [ 
        { "type": "phone", "number": req.query.to } 
      ]
    }
  ]);
});

app.all('/voice/event', (req, res) => {
  console.log('EVENT:');
  console.dir(req.body);
  console.log('---');
  res.sendStatus(200);
});

if(subdomain == "SUBDOMAIN") {
  console.log('\n\t🚨🚨🚨 Please change the SUBDOMAIN value');
  return false;
}
if(vonageNumber == "NUMBER") {
  console.log('\n\t🚨🚨🚨 Please change the NUMBER value');
  return false;
}
app.listen(3000);

const localtunnel = require('localtunnel');
(async () => {
  const tunnel = await localtunnel({ 
      subdomain: subdomain, 
      port: 3000
    });
  console.log(`App available at: ${tunnel.url}`);
})();
```

> **NOTE:** Please remember to replace `SUBDOMAIN` with a random string of your choice between 4 and 20 alphanumeric characters (letters and numbers, not underscores or dashes) and `NUMBER` with your Vonage number in [E.164](https://en.wikipedia.org/wiki/E.164) format (e.g. 447700900000).


There are 2 parts in the server code above:


### The Express server

The first part creates an `Express` server and makes it available locally on port `3000`. The server exposes 2 paths:

1. `/voice/answer` is the `answer_url` we mentioned above. It sends back a `JSON` response containing the destination number for the call. 
   
    Notice, that the `number` is extracted from the `req.query.to` parameter that Vonage is sending as part of the request. The dynamically built NCCO then forwards the call to the destination phone using a `connect` action.

2. The second one, `/voice/event`, you will set as destination for Vonage to notify you of everything happening during the call - - we call this the `event_url`.


### The `localtunnel`  integration

The second part of the server code above, exposes the `Express` server so it will be accessible by the Vonage servers.

> **NOTE:** `localtunnel` is a JavaScript library that exposes your localhost to the world for painless testing and sharing! No need to mess with DNS or deploy to have others test out your changes.


## Start the server

You can now start the server by running, in the terminal, the following command:

``` bash
node server.js
```

A notice will be displayed telling you the server is now available:

```
App available at: https://SUBDOMAIN.loca.lt
```

Please keep the terminal window handy as you will need the URL in the next step.
