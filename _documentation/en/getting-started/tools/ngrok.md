---
title: Ngrok
description: This topic shows you how to test your Vonage application locally using Ngrok.
navigation_weight: 2
---

# Testing with Ngrok

When your users interact with Vonage, for example by calling a Vonage number, Vonage needs to notify your application so that it can take required action. In the case of a Voice application, this might be to play a text-to-speech message to the caller when Vonage answers the call. Vonage can also keep your application notified of various events that take place during the call, such as when the caller hangs up. These notifications from Vonage are implemented using [webhooks](/concepts/guides/webhooks).

An issue is when you are testing your application locally on your development machine, your computer is unable to receive these webhook notifications as it is not on the public Internet.

One way to work around this problem is to use a free tool called [Ngrok](https://ngrok.com/). Ngrok creates a secure tunnel connecting the Internet to your locally-running application. Notifications from Vonage will then be routed through Ngrok to your local machine.

To get started with Ngrok, follow these steps:

1) Download and install Ngrok using the [instructions](https://ngrok.com/download) for your platform.

2) Start your application locally, and check which port it uses. For example, if you usually use `http://localhost:3000` to access your application, then the port is 3000.

3) Create an Ngrok tunnel to this port with a command such as `ngrok http 3000`. You will see something similar to the following:

``` shell
ngrok by @inconshreveable (Ctrl+C to quit)

Session Status online
Account YourUsername (Plan: Free)
Version 2.3.35
Region United States (us)
Web Interface http://127.0.0.1:4040
Forwarding http://abcdef1.ngrok.io -> http://localhost:3000
Forwarding https://abcdef1.ngrok.io -> http://localhost:3000

Connections ttl opn rt1 rt5 p50 p90
```

4) This command will show the URL of the tunnel, which will look similar to `https://abcdef1.ngrok.io`. Copy this URL.

5) [Configure your Vonage API webhooks](#setting-webhook-endpoints) to point to this URL.

Note that each time you start up the Ngrok tunnel, you will receive a new URL so don't forget to update the webhooks configuration accordingly!

## Usage with IIS Express

IIS Express rejects inbound requests from Ngrok if the host header does not match the `bindingInformation` pattern in its configuration. Therefore you will need to configure Ngrok to change the host name in the header to match the IIS Express configuration.

This can be done using Ngrok's `host-header` option. The following command demonstrates how to do this for port 3000:

``` shell
ngrok http 3000 --host-header="localhost:3000"
```

If you are doing this for your testing and need the original host for something, you can retrieve it from the `X-Original-Host` header.

### Configure IIS Express for the correct port

Validate that IIS Express is listening on port 3000, or whichever port you've decided to configure Ngrok to, by opening up your `properties/launchSettings.json` and making sure the port number in the `iisExpress` object's `applicationUrl` field matches the port you've configured Ngrok to forward to. You should also disable SSL on IIS Express by setting the `sslPort` to `0`. See the following valid configuration for IIS Express from `launchSettings.json`:

``` json
"iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3000/",
      "sslPort": 0
    }
  }
```

## Ngrok plans

Paid Ngrok plans can reserve tunnel names and choose which one to use when they start up, so you can avoid having to reconfigure your webhooks. If you have a paid Ngrok account, you can start on your own domain using a command such as:

``` shell
ngrok http -subdomain=mydomain 3000
```

In this case you will something similar to the following:

``` shell
ngrok by @inconshreveable (Ctrl+C to quit)

Session Status online
Account YourUsername (Plan: Basic)
Version 2.3.35
Region United States (us)
Web Interface http://127.0.0.1:4040
Forwarding http://mydomain.ngrok.io -> http://localhost:3000
Forwarding https://mydomain.ngrok.io -> http://localhost:3000

Connections ttl opn rt1 rt5 p50 p90
```

This has the advantage that each time you start Ngrok you won't need to reconfigure your webhooks.

## Ngrok web interface

In addition to the tunnel URL displayed in the command output, you will also see a URL for the "Web Interface" (`http://127.0.0.1:4040`). The web interface allows you to examine the details of all the requests received by the tunnel and the responses returned. This is a very useful debugging tool.

In the web interface you can also use the "Replay" button to repeat a request rather than having to send yourself lots of text messages or continuously reproduce whatever event your webhook code is responding to. These features make testing your application's response to webhooks much more convenient.

## See also

* [Ngrok blog post](https://www.nexmo.com/blog/2017/07/04/local-development-nexmo-ngrok-tunnel-dr)
