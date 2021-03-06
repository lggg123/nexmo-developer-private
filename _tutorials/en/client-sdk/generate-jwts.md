---
title: Generate JWTs
description: In this step you learn how to generate valid JWTs for each User in your Conversation
---

# Generate JWTs

You need to generate a JWT for each user. The JWT is used to authenticate the user.

In the following code remember to replace the `MY_APP_ID`, `USER1_NAME` and `USER2_NAME` variables with your own values:

```shell
vonage jwt --key_file=./private.key --acl='{"paths":{"/*/users/**":{},"/*/conversations/**":{},"/*/sessions/**":{},"/*/devices/**":{},"/*/image/**":{},"/*/media/**":{},"/*/applications/**":{},"/*/push/**":{},"/*/knocking/**":{},"/*/legs/**":{}}}' --subject=USER1_NAME --app_id=APP_ID

vonage jwt --key_file=./private.key --acl='{"paths":{"/*/users/**":{},"/*/conversations/**":{},"/*/sessions/**":{},"/*/devices/**":{},"/*/image/**":{},"/*/media/**":{},"/*/applications/**":{},"/*/push/**":{},"/*/knocking/**":{},"/*/legs/**":{}}}' --subject=USER2_NAME --app_id=APP_ID
```

The above command sets the expiry of the JWT to one day from now, which is the maximum.

Make a note of the JWT you generated for each user.

> **NOTE**: In a production environment, your application should expose an endpoint that generates a JWT for each client request.

## Using the web interface

Alternatively, you can use our <a href="/jwt" target="_blank">online JWT generator</a> with the following parameters to generate a JWT.

**Application ID:** `MY_APP_ID`<br />
**Sub:** `USER1_NAME` / `USER2_NAME`<br />
**ACL:**

```json
{
  "paths": {
    "/*/users/**": {},
      "/*/conversations/**": {},
      "/*/sessions/**": {},
      "/*/devices/**": {},
      "/*/image/**": {},
      "/*/media/**": {},
      "/*/applications/**": {},
      "/*/push/**": {},
      "/*/knocking/**": {},
      "/*/legs/**": {}
  }
}
```

## Further information

* [JWT guide](/concepts/guides/authentication#json-web-tokens-jwt)
