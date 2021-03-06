---
title: FreePBX
description: How to setup Vonage SIP with FreePBX
---

# FreePBX

Below we provide example configurations for using Vonage's SIP service with [FreePBX](http://www.freepbx.org).

> Visit the [Vonage Knowledge Base](https://help.nexmo.com/hc/en-us/articles/115004859247-Which-IP-addresses-should-I-whitelist-in-order-to-receive-voice-traffic-from-Nexmo) to obtain the current list of IP addresses.

## Inbound configuration

```text
host=5.10.112.121
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833

host=5.10.112.122
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833

host=119.81.44.6
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833

host=119.81.44.7
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833

host=169.60.141.29
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833

host=169.60.141.30
type=friend
insecure=port,invite
;Add your codec list here.
; Note: Use "ulaw" for US only, "alaw" for the rest of the world.
allow=ulaw,alaw,g729
dtmfmode=rfc2833
```

## Outbound configuration

```text
host=sip.nexmo.com
  type=friend
  insecure=port,invite
  qualify=yes
  ;Add your codec list here.
  ;Note: Use "ulaw" for US only, "alaw" for the rest of the world.
  allow=ulaw,alaw,g729
  dtmfmode=rfc2833

  username=<key>
  fromuser=<long_virtual_number>
  secret=<secret>

  Register String
  <key>:<secret>@sip.nexmo.com
```

## PJSIP configuration

For PJSIP configuration use the following information:

| Configuration item | Value |
|----|----|
| Authentication | Outbound |
| Registration | None |
| SIP Server | sip.nexmo.com |
| Username | API Key |
| Secret | API Secret |
| From Domain | sip.nexmo.com |
| DTMF Mode | RFC4733 |
| Codecs | ulaw, alaw, g729 |
